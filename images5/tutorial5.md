# Tutorial 5

## Task 1: View Your Cookies

![Browser Cookies](cookies.png)

### Types of Information Stored by Cookies:
* **User Identity & State:** The `dotcom_user` cookie stores my GitHub username (`cmorris40`) to maintain an active profile state, while `logged_in: yes` indicates whether the session is authenticated.
* **Interface Preferences:** The `color_mode` and `preferred_color_mode` cookies store UI settings (such as dark mode preferences) so the site renders correctly across page loads.
* **Session & Security Tokens:** The `_gh_sess` and `_Host-` cookies store cryptographically signed session tokens. They have the `HttpOnly` and `Secure` attributes enabled to prevent client-side JavaScript access and defend against cross-site scripting (XSS).
* **Analytics & Performance Tracking:** Cookies such as `_octo` and `_dd_s` track site performance metrics, client analytics, and navigation logs across GitHub services.
