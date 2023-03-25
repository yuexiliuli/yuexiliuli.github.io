---
title: NET MVC身份验证及权限管理
urlname: yr4fng
date: '2021-05-15 21:27:04 +0800'
tags: [.NET]
categories: [.NET]
---

# 一、项目结构

![1589169651574.png](https://272y4n7101.goho.co/i/2023/03/22/641ad9ceeced4.png)

# 二、编写代码

## （1）创建实体类

### AccountModels .cs

```csharp
namespace WebApplication1.Models
{

    #region Models
    [PropertiesMustMatch("NewPassword", "ConfirmPassword", ErrorMessage = "The new password and confirmation password do not match.")]
    public class ChangePasswordModel
    {
        [Required]
        [DataType(DataType.Password)]
        [DisplayName("Current password")]
        public string OldPassword { get; set; }

        [Required]
        [ValidatePasswordLength]
        [DataType(DataType.Password)]
        [DisplayName("New password")]
        public string NewPassword { get; set; }

        [Required]
        [DataType(DataType.Password)]
        [DisplayName("Confirm new password")]
        public string ConfirmPassword { get; set; }
    }

    public class LogOnModel
    {
        [Required]
        [DisplayName("User name")]
        public string UserName { get; set; }

        [Required]
        [DataType(DataType.Password)]
        [DisplayName("Password")]
        public string Password { get; set; }

        [DisplayName("Remember me?")]
        public bool RememberMe { get; set; }
    }

    [PropertiesMustMatch("Password", "ConfirmPassword", ErrorMessage = "The password and confirmation password do not match.")]
    public class RegisterModel
    {
        [Required]
        [DisplayName("User name")]
        public string UserName { get; set; }

        [Required]
        [DataType(DataType.EmailAddress)]
        [DisplayName("Email address")]
        public string Email { get; set; }

        [Required]
        [ValidatePasswordLength]
        [DataType(DataType.Password)]
        [DisplayName("Password")]
        public string Password { get; set; }

        [Required]
        [DataType(DataType.Password)]
        [DisplayName("Confirm password")]
        public string ConfirmPassword { get; set; }
    }
    #endregion

    #region Services
    // The FormsAuthentication type is sealed and contains static members, so it is difficult to
    // unit test code that calls its members. The interface and helper class below demonstrate
    // how to create an abstract wrapper around such a type in order to make the AccountController
    // code unit testable.

    public interface IMembershipService
    {
        int MinPasswordLength { get; }

        bool ValidateUser(string userName, string password);
        MembershipCreateStatus CreateUser(string userName, string password, string email);
        bool ChangePassword(string userName, string oldPassword, string newPassword);
    }

    public class AccountMembershipService : IMembershipService
    {
        private readonly MembershipProvider _provider;

        public AccountMembershipService()
            : this(null)
        {
        }

        public AccountMembershipService(MembershipProvider provider)
        {
            _provider = provider ?? Membership.Provider;
        }

        public int MinPasswordLength
        {
            get
            {
                return _provider.MinRequiredPasswordLength;
            }
        }

        public bool ValidateUser(string userName, string password)
        {
            if (String.IsNullOrEmpty(userName)) throw new ArgumentException("Value cannot be null or empty.", "userName");
            if (String.IsNullOrEmpty(password)) throw new ArgumentException("Value cannot be null or empty.", "password");

            return _provider.ValidateUser(userName, password);
        }

        public MembershipCreateStatus CreateUser(string userName, string password, string email)
        {
            if (String.IsNullOrEmpty(userName)) throw new ArgumentException("Value cannot be null or empty.", "userName");
            if (String.IsNullOrEmpty(password)) throw new ArgumentException("Value cannot be null or empty.", "password");
            if (String.IsNullOrEmpty(email)) throw new ArgumentException("Value cannot be null or empty.", "email");

            MembershipCreateStatus status;
            _provider.CreateUser(userName, password, email, null, null, true, null, out status);
            return status;
        }

        public bool ChangePassword(string userName, string oldPassword, string newPassword)
        {
            if (String.IsNullOrEmpty(userName)) throw new ArgumentException("Value cannot be null or empty.", "userName");
            if (String.IsNullOrEmpty(oldPassword)) throw new ArgumentException("Value cannot be null or empty.", "oldPassword");
            if (String.IsNullOrEmpty(newPassword)) throw new ArgumentException("Value cannot be null or empty.", "newPassword");

            // The underlying ChangePassword() will throw an exception rather
            // than return false in certain failure scenarios.
            try
            {
                MembershipUser currentUser = _provider.GetUser(userName, true /* userIsOnline */);
                return currentUser.ChangePassword(oldPassword, newPassword);
            }
            catch (ArgumentException)
            {
                return false;
            }
            catch (MembershipPasswordException)
            {
                return false;
            }
        }
    }

    public interface IFormsAuthenticationService
    {
        void SignIn(string userName, bool createPersistentCookie);
        void SignOut();
    }

    public class FormsAuthenticationService : IFormsAuthenticationService
    {
        public void SignIn(string userName, bool createPersistentCookie)
        {
            if (String.IsNullOrEmpty(userName)) throw new ArgumentException("Value cannot be null or empty.", "userName");

            FormsAuthentication.SetAuthCookie(userName, createPersistentCookie);
        }

        public void SignOut()
        {
            FormsAuthentication.SignOut();
        }
    }
    #endregion

    #region Validation
    public static class AccountValidation
    {
        public static string ErrorCodeToString(MembershipCreateStatus createStatus)
        {
            // See http://go.microsoft.com/fwlink/?LinkID=177550 for
            // a full list of status codes.
            switch (createStatus)
            {
                case MembershipCreateStatus.DuplicateUserName:
                    return "Username already exists. Please enter a different user name.";

                case MembershipCreateStatus.DuplicateEmail:
                    return "A username for that e-mail address already exists. Please enter a different e-mail address.";

                case MembershipCreateStatus.InvalidPassword:
                    return "The password provided is invalid. Please enter a valid password value.";

                case MembershipCreateStatus.InvalidEmail:
                    return "The e-mail address provided is invalid. Please check the value and try again.";

                case MembershipCreateStatus.InvalidAnswer:
                    return "The password retrieval answer provided is invalid. Please check the value and try again.";

                case MembershipCreateStatus.InvalidQuestion:
                    return "The password retrieval question provided is invalid. Please check the value and try again.";

                case MembershipCreateStatus.InvalidUserName:
                    return "The user name provided is invalid. Please check the value and try again.";

                case MembershipCreateStatus.ProviderError:
                    return "The authentication provider returned an error. Please verify your entry and try again. If the problem persists, please contact your system administrator.";

                case MembershipCreateStatus.UserRejected:
                    return "The user creation request has been canceled. Please verify your entry and try again. If the problem persists, please contact your system administrator.";

                default:
                    return "An unknown error occurred. Please verify your entry and try again. If the problem persists, please contact your system administrator.";
            }
        }
    }

    [AttributeUsage(AttributeTargets.Class, AllowMultiple = true, Inherited = true)]
    public sealed class PropertiesMustMatchAttribute : ValidationAttribute
    {
        private const string _defaultErrorMessage = "'{0}' and '{1}' do not match.";
        private readonly object _typeId = new object();

        public PropertiesMustMatchAttribute(string originalProperty, string confirmProperty)
            : base(_defaultErrorMessage)
        {
            OriginalProperty = originalProperty;
            ConfirmProperty = confirmProperty;
        }

        public string ConfirmProperty { get; private set; }
        public string OriginalProperty { get; private set; }

        public override object TypeId
        {
            get
            {
                return _typeId;
            }
        }

        public override string FormatErrorMessage(string name)
        {
            return String.Format(CultureInfo.CurrentUICulture, ErrorMessageString,
                OriginalProperty, ConfirmProperty);
        }

        public override bool IsValid(object value)
        {
            PropertyDescriptorCollection properties = TypeDescriptor.GetProperties(value);
            object originalValue = properties.Find(OriginalProperty, true /* ignoreCase */).GetValue(value);
            object confirmValue = properties.Find(ConfirmProperty, true /* ignoreCase */).GetValue(value);
            return Object.Equals(originalValue, confirmValue);
        }
    }

    [AttributeUsage(AttributeTargets.Field | AttributeTargets.Property, AllowMultiple = false, Inherited = true)]
    public sealed class ValidatePasswordLengthAttribute : ValidationAttribute
    {
        private const string _defaultErrorMessage = "'{0}' must be at least {1} characters long.";
        private readonly int _minCharacters = Membership.Provider.MinRequiredPasswordLength;

        public ValidatePasswordLengthAttribute()
            : base(_defaultErrorMessage)
        {
        }

        public override string FormatErrorMessage(string name)
        {
            return String.Format(CultureInfo.CurrentUICulture, ErrorMessageString,
                name, _minCharacters);
        }

        public override bool IsValid(object value)
        {
            string valueAsString = value as string;
            return (valueAsString != null && valueAsString.Length >= _minCharacters);
        }
    }
    #endregion

}
```

###

### User

```csharp
namespace WebApplication1.Models
{
    public class User
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Password { get; set; }
        public string[] Roles { get; set;  }
    }
}
```

## （2）Repository:对数据进行操作

### UserRepository.cs

没有把数据放在数据库中，这里采用模拟数据的方式。

```csharp
namespace WebApplication1.Repositoty
{
    public class UserRepository
    {
        private static User[] usersForTest = new[]{
        new User{ ID = 1, Name = "bob", Password = "bob", Roles = new []{"employee"}},
        new User{ ID = 2, Name = "tom", Password = "tom", Roles = new []{"manager"}},
        new User{ ID = 3, Name = "admin", Password = "admin", Roles = new[]{"admin"}},
    };

        public bool ValidateUser(string userName, string password)
        {
            return usersForTest
                .Any(u => u.Name == userName && u.Password == password);
        }

        public string[] GetRoles(string userName)
        {
            return usersForTest
                .Where(u => u.Name == userName)
                .Select(u => u.Roles)
                .FirstOrDefault();
        }

        public User GetByNameAndPassword(string name, string password)
        {
            return usersForTest
                .FirstOrDefault(u => u.Name == name && u.Password == password);
        }
    }
}
```

## （3）配置 Web.config

在 web.config 文件中，

<system.web>配置节用于对验证进行配置。

为节点提供 mode="Forms"属性

可以启用 Forms Authentication。

当前项目的配置节如下所示：

```html
<system.web>
  <authentication mode="Forms">
    <forms
      loginUrl="~/Account/LogOn"
      defaultUrl="~/Home/Index"
      protection="All"
    />
  </authentication>
  ...... ......
</system.web>
```

- loginUrl：表示登录页面的连接，如果在未登录(授权)的状态下访问需要权限的页面，则跳转到~/Account/LogOn
- defaultUrl：登陆后默认跳转的页面
- protection：表示对哪些页面生效

## （4）修改 Global.asax

```csharp
namespace WebApplication1
{
    public class MvcApplication : System.Web.HttpApplication
    {

        protected void Application_Start()/*默认的配置*/
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }

        //此方式将用户的角色保存至用户 Cookie，使用到了 FormsAuthenticationTicket。

        /// <summary>
        /// 添加构造函数
        /// </summary>
        public MvcApplication()
        {
            AuthorizeRequest += new EventHandler(MvcApplication_AuthorizeRequest);
        }
        //权限请求
        void MvcApplication_AuthorizeRequest(object sender, EventArgs e)
        {
            var id = Context.User.Identity as FormsIdentity;
            if (id != null && id.IsAuthenticated)//这里判断访问者是否成功进行了身份验证
            {
                //通过逗号分割，多个权限
                var roles = id.Ticket.UserData.Split(',');
                //得到请求
                Context.User = new GenericPrincipal(id, roles);
            }
        }
    }
}
```

## （5）Controller 控制器

### AccountController

```csharp
public class AccountController : Controller
    {
        private UserRepository repository = new UserRepository();

        public ActionResult LogOn()
        {
            return View();
        }

        [HttpPost]
        public ActionResult LogOnss(LogOnModel model)
        {
            //if (ModelState.IsValid)
            //{
                //找用户
                User user = repository.GetByNameAndPassword(model.UserName, model.Password);
                //找到
                if (user != null)
                {
                //提供对Form的身份验证用于确定用户身份
                    FormsAuthenticationTicket ticket = new FormsAuthenticationTicket(
                        1,          //版本
                        user.Name,  //名称
                        DateTime.Now,//发行时间
                        DateTime.Now.Add(FormsAuthentication.Timeout),//到期时间
                        model.RememberMe,//持久化
                        user.Roles.Aggregate((i, j) => i + "," + j)//特定于用户的数据
                        );
                    HttpCookie cookie = new HttpCookie(//创建cookie
                        FormsAuthentication.FormsCookieName,//name
                        FormsAuthentication.Encrypt(ticket));//value
                cookie.Expires = DateTime.Now.AddSeconds(10);//十秒钟过期
                    Response.Cookies.Add(cookie);//添加cookie

                    //if (!String.IsNullOrEmpty(returnUrl)) return Redirect(returnUrl);else
                return RedirectToAction("Index", "Home");//登录成功后跳转
                }
                else
                    ModelState.AddModelError("", "用户名或密码不正确！");
            //}

            return Content("success");
        }

        public ActionResult LogOff()
        {
            FormsAuthentication.SignOut();
            return RedirectToAction("Index", "Home");
        }
    }
```

### HomeController

权限放在了 User 表 Roles 字段中，可以在 UserRepository 中修改其值。

```csharp
public class HomeController : Controller
    {
        //有一种权限即可登录employee or manager
        [Authorize(Roles = "employee,manager")]
        public ActionResult Index()
        {
            return View();
        }

        //admin权限
        [Authorize(Roles = "admin")]
        public ActionResult About()
        {
            ViewBag.Message = "Your application description page.";

            return View();
        }
        [Authorize(Roles = "employee")]
        public ActionResult Contact()
        {
            ViewBag.Message = "Your contact page.";

            return View();
        }
    }
```

# 三、Views 下面的视图

LogOn.cshtml

```html
<div class="well well-sm" style="width:700px;margin-left:200px">
  <div class="modal-body">
    <div class="row">
      <div class="col-md-6 col-md-offset-3">
        <h1 style="width:700px;margin-left:50px">账 户 登 录</h1>

        <form class="form-group" action="/Account/LogOnss" method="post">
          <label>name</label>
          <input type="text" name="UserName" class="form-control" />
          <label>password</label>
          <input type="password" name="Password" class="form-control" />

          <input
            value="登          录"
            class="btn btn-primary"
            type="submit"
            style="width:420px"
          />
        </form>
      </div>
    </div>
  </div>
</div>
```

# 参考博客

### [MVC 身份验证及权限管理](https://www.cnblogs.com/asks/p/4372783.html)

### [ ASP.NET MVC 中的 Global.asax 文件](https://www.cnblogs.com/lgxlsm/p/5573088.html)

### [MVC 身份验证及权限管理](https://www.cnblogs.com/jin-/p/9688732.html)
