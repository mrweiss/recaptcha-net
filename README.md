<h1>reCAPTCHA library for .NET</h1>

reCAPTCHA for .NET is a library that allows a developer to easily integrate Google's reCAPTCHA service (the most popular captcha used by millions of sites) in an ASP.NET Web Forms or ASP.NET MVC web application. 

The library is one of the most solid captcha libraries with the best documentation. Unlike most other reCAPTCHA libraries, it supports both ASP.NET Web Forms and ASP.NET MVC.

<h2>Features</h2>

The primary features of the library are:
<ul>
<li>Render recaptcha control (HTML) with appropriate options for pre-defined themes and culture (language).</li>
<li>Verify user's answer to recaptcha's challenge.</li>
<li>Supports ASP.NET Web Forms and ASP.NET MVC.</li>
</ul>

<h2>Creating a reCAPTCHA Keys</h2>
Before you can use reCAPTCHA in your web application, you must first create a reCAPTCHA key (a pair of public and private keys). Creating reCAPTCHA key is very straight-forward. The following are the steps:
<ol>
<li>Go to the Google's <a href="https://www.google.com/recaptcha">Recaptcha</a> site.</li>
<li>Click on the <strong>Get reCAPTCHA</strong> button. You will be required to login with your Google account.</li>
<li>Enter a label for this reCAPTCHA and the domain of your web application. You can enter more than one domain if you want to.</li>
<li>Expand <strong>Keys</strong> under the <strong>Adding reCAPTCHA to your site</strong> section. Note down your <strong>Site Key</strong> and <strong>Secret Key</strong>.</li>
</ol>

<h2>Installing reCAPTCHA for .NET</h2>
<h3>Recaptcha Nuget Package</h3>
The best and the recommended way to install the latest version of reCAPTCHA for .NET is through Nuget. From the <a href="http://docs.nuget.org/consume/package-manager-console">Nuget's Package Manager Console</a> in your Visual Studio .NET IDE, simply execute the following command:

```
PM> Install-Package RecaptchaNet
```

If the Package Manager Console is not visible in your Microsoft Visual Studio IDE, click on the <strong>Tools > Library Package Manager > Package Manager Console</strong> menu.

<strong>Note</strong>: Nuget is a Visual Studio extension and it needs to be installed before you can use Nuget packages in your Visual Studio projects. You can download and install <a href="https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c">Nuget</a> from here.

<h2>How to Use reCAPTCHA in an ASP.NET Web Forms Application</h2>
Add the following line just under the Page directive in your .aspx or .ascx file:

```
<%@ Register Assembly="Recaptcha.Web" Namespace="Recaptcha.Web.UI.Controls" TagPrefix="cc1" %>
```

Then at the desired line in the same file add the Recaptcha control as follows:

```
<cc1:Recaptcha ID="Recaptcha1" PublicKey="6LdkfdwSAAAAABL1099CPTr6473FQFXNLR_04Bb5" PrivateKey="6LdkfdwSAAAAAFC8jtUY44wuhC9lmDlFrL6qMAAh" runat="server" />
```

Rather than setting the recaptcha key of the control through its PublicKey and PrivateKey properties, you can set them in your web.config file instead:
<h3>How to Set Recpatcha Key in Web.config File</h3>
After you set the private and public keys in your web.config file, all you need in your web form is this following piece of code:

```
<cc1:Recaptcha ID="Recaptcha1" runat="server" />
```

By default, the theme of the Recaptcha control is Red. However, you can change this default theme to one of the other three themes if you like. Those themes are: Blackglass, White, and Clean. Theme can be set by using the <strong>RecaptchaTheme</strong> enum. The following is an example:

```
<cc1:Recaptcha ID="Recaptcha1" Theme="RecaptchaTheme.Clean" runat="server" />
```

<h4>Add the Recaptcha Control to the Visual Studio Toolbox</h4>

Instead of writing the above code manually, you can easily drag and drop the same Recaptcha control from the Visual Studio Toolbox onto your page designer just like the way you would do for other standard ASP.NET controls. However, you would need to add the Recaptcha control to the Toolbox first. Simply, right click on the Toolbox and select Choose Items... from the context menu and then under the .NET Framework Components tab click on the Browse button and locate the <strong>Recaptcha.Web.dll</strong> assembly.

<h3>Verify User's Response to Recaptcha Challenge</h3>

When your end-user submits the form that contains the Recaptcha control, you obviously would want to verify whether the user's answer was valid based on what was displayed in the recaptcha image. It is very easy to do with one or two lines.

First of all as expected, import the namespace Recaptcha.Web in your code-behind file:

```
using Recaptcha.Web;
```

To verify whether the user's answer is correct, call the control's Verify() method which returns RecaptchaVerificationResult. You can also use the control's Response property to check what the actual answer is. Generally, you would want to use the Response property to check if the user provided a blank response which of course is always wrong:

```
if (String.IsNullOrEmpty(Recaptcha1.Response))
{
    lblMessage.Text = "Captcha cannot be empty.";
}
else
{
    RecaptchaVerificationResult result = await Recaptcha1.Verify();

    if (result == RecaptchaVerificationResult.Success)
    {
        Response.Redirect("Welcome.aspx");
    }
    if (result == RecaptchaVerificationResult.IncorrectCaptchaSolution)
    {
        lblMessage.Text = "Incorrect captcha response.";
    }
    else
    {
        lblMessage.Text = "Some other problem with captcha.";
    }
}
```

Instead of calling the Verify() method, you can call the VerifyTaskAsync() method to verify the user's response asynchronously which at the same time can be used along with the new await keyword:

```
if (String.IsNullOrEmpty(Recaptcha1.Response))
{
    lblMessage.Text = "Captcha cannot be empty.";
}
else
{
    RecaptchaVerificationResult result = await Recaptcha1.VerifyTaskAsync();

    if (result == RecaptchaVerificationResult.Success)
    {
        Response.Redirect("Welcome.aspx");
    }
    if (result == RecaptchaVerificationResult.IncorrectCaptchaSolution)
    {
        lblMessage.Text = "Incorrect captcha response.";
    }
    else
    {
        lblMessage.Text = "Some other problem with captcha.";
    }
}
```


