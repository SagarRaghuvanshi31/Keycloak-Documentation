# Keycloak

Keycloak is an open-source identity and access management (IAM) tool that provides secure authentication and authorization for applications. It enables Single Sign-On (SSO), allowing users to log in once and access multiple services without re-entering credentials.

### Key Features:

User Authentication – Supports passwords, social logins (Google, Facebook), and multi-factor authentication (MFA).

User Management – Allows users to manage their accounts, reset passwords, and update profiles.

Access Control – Provides role-based permissions to restrict access.

Enterprise Integration – Works with LDAP, Active Directory, and OAuth 2.0/OpenID Connect for seamless authentication.

### Keycloakify Overview

Keycloakify is a tool for creating custom Keycloak themes, enabling you to modify the appearance and behavior of Keycloak's user interfaces. This includes:

- **Login Theme**: The UI for login and registration pages, displayed to users when they attempt to log in or sign up.
- **Account Theme**: The account management interface, where users can update their email, change their password, and manage other account settings.
- **Email Theme**: The templates used by Keycloak for automated emails, such as email confirmation or password reset notifications.
- **Admin Theme**: The Admin Console interface, used by administrators to configure Keycloak.

For a visual preview of these UIs as they appear with Keycloak's built-in theme, visit the **Storybook demonstration**.

### Why Choose Keycloakify?

You might be wondering why you would need a third-party tool like Keycloakify to create your custom UIs instead of relying solely on Keycloak's built-in theming system. Here are a few reasons:

- **Leverage Modern Frontend Technologies**: Keycloakify enables you to use TypeScript, React, Angular, Svelte, and any styling solution or component library you prefer, such as Tailwind, MUI, shadcn/ui, or plain CSS.
- **Streamlined Testing**: Keycloakify makes it easy to test your theme both inside and outside Keycloak, with hot reloading for a smoother development experience.
- **Automated Theme Bundling**: Keycloakify bundles your theme into a JAR file, ready to import directly into Keycloak.
- **Community Support**: If you're stuck or need guidance, reach out through our **Discord channel** or **GitHub issues**.

# Customizing Keycloak Login Page Theme  

This guide explains how to customize the Keycloak login page step by step.  
We will use the Keycloakify starter template, set up Keycloak using Docker,  
integrate Storybook for UI testing, modify the login UI, and deploy the custom theme.  

## 1. Clone the Starter Template  

### Step 1: Clone the Repository  
First, clone the Keycloakify starter template from GitHub:  

```sh
git clone https://github.com/keycloakify/keycloakify-starter.git
```

### Step 2: Install Dependencies

After cloning, navigate into the project directory and install dependencies:

```
cd keycloakify-starter
npm install
```

## 2. Running Keycloak with Docker

We will run Keycloak as a Docker container to make testing easier.

### Step 1: Start Keycloak Using Docker

Run the following command to start a Keycloak instance:

```
docker run -d \
  --name keycloak \
  -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:latest \
  start-dev
```
 Keycloak Admin Console will be available at: http://localhost:8080
 
 Admin Username: admin
 
 Admin Password: admin

## 3. Running Keycloakify & Developing the Theme

To start developing the theme, use the following command:

```
npx keycloakify start-keycloak
```

This will launch Keycloak with your custom theme.

## 4. Customizing the Template.tsx

To modify the template, update the Template.tsx file.

```
import { useEffect } from "react";
import { clsx } from "keycloakify/tools/clsx";
import { kcSanitize } from "keycloakify/lib/kcSanitize";
import type { TemplateProps } from "keycloakify/login/TemplateProps";
import { getKcClsx } from "keycloakify/login/lib/kcClsx";
import { useSetClassName } from "keycloakify/tools/useSetClassName";
import { useInitialize } from "keycloakify/login/Template.useInitialize";
import type { I18n } from "./i18n";
import type { KcContext } from "./KcContext";
import './main.css';


export default function Template(props: TemplateProps<KcContext, I18n>) {
    const {
        displayInfo = false,
        displayMessage = true,
        displayRequiredFields = false,
        headerNode,
        socialProvidersNode = null,
        infoNode = null,
        documentTitle,
        bodyClassName,
        kcContext,
        i18n,
        doUseDefaultCss,
        classes,
        children
    } = props;

    const { kcClsx } = getKcClsx({ doUseDefaultCss, classes });

    const { msg, msgStr, currentLanguage, enabledLanguages } = i18n;

    const { realm, auth, url, message, isAppInitiatedAction } = kcContext;

    useEffect(() => {
        document.title = documentTitle ?? msgStr("loginTitle", kcContext.realm.displayName);
    }, []);

    useSetClassName({
        qualifiedName: "html",
        className: kcClsx("kcHtmlClass")
    });

    useSetClassName({
        qualifiedName: "body",
        className: bodyClassName ?? kcClsx("kcBodyClass")
    });

    const { isReadyToRender } = useInitialize({ kcContext, doUseDefaultCss });

    if (!isReadyToRender) {
        return null;
    }

    return (
        <div className={kcClsx("kcLoginClass")}>
            <div id="kc-header" className={kcClsx("kcHeaderClass")}>
                <div id="kc-header-wrapper" className={kcClsx("kcHeaderWrapperClass")}>
                    {msg("loginTitleHtml", realm.displayNameHtml)}
                </div>
            </div>
            <div className={kcClsx("kcFormCardClass")}>
                <header className={kcClsx("kcFormHeaderClass")}>
                    {enabledLanguages.length > 1 && (
                        <div className={kcClsx("kcLocaleMainClass")} id="kc-locale">
                            <div id="kc-locale-wrapper" className={kcClsx("kcLocaleWrapperClass")}>
                                <div id="kc-locale-dropdown" className={clsx("menu-button-links", kcClsx("kcLocaleDropDownClass"))}>
                                    <button
                                        tabIndex={1}
                                        id="kc-current-locale-link"
                                        aria-label={msgStr("languages")}
                                        aria-haspopup="true"
                                        aria-expanded="false"
                                        aria-controls="language-switch1"
                                    >
                                        {currentLanguage.label}
                                    </button>
                                    <ul
                                        role="menu"
                                        tabIndex={-1}
                                        aria-labelledby="kc-current-locale-link"
                                        aria-activedescendant=""
                                        id="language-switch1"
                                        className={kcClsx("kcLocaleListClass")}
                                    >
                                        {enabledLanguages.map(({ languageTag, label, href }, i) => (
                                            <li key={languageTag} className={kcClsx("kcLocaleListItemClass")} role="none">
                                                <a role="menuitem" id={`language-${i + 1}`} className={kcClsx("kcLocaleItemClass")} href={href}>
                                                    {label}
                                                </a>
                                            </li>
                                        ))}
                                    </ul>
                                </div>
                            </div>
                        </div>
                    )}
                    {(() => {
                        const node = !(auth !== undefined && auth.showUsername && !auth.showResetCredentials) ? (
                            <h1 id="kc-page-title">{headerNode}</h1>
                        ) : (
                            <div id="kc-username" className={kcClsx("kcFormGroupClass")}>
                                <label id="kc-attempted-username">{auth.attemptedUsername}</label>
                                <a id="reset-login" href={url.loginRestartFlowUrl} aria-label={msgStr("restartLoginTooltip")}>
                                    <div className="kc-login-tooltip">
                                        <i className={kcClsx("kcResetFlowIcon")}></i>
                                        <span className="kc-tooltip-text">{msg("restartLoginTooltip")}</span>
                                    </div>
                                </a>
                            </div>
                        );

                        if (displayRequiredFields) {
                            return (
                                <div className={kcClsx("kcContentWrapperClass")}>
                                    <div className={clsx(kcClsx("kcLabelWrapperClass"), "subtitle")}>
                                        <span className="subtitle">
                                            <span className="required">*</span>
                                            {msg("requiredFields")}
                                        </span>
                                    </div>
                                    <div className="col-md-10">{node}</div>
                                </div>
                            );
                        }

                        return node;
                    })()}
                </header>
                <div id="kc-content">
                    <div id="kc-content-wrapper">
                        {/* App-initiated actions should not see warning messages about the need to complete the action during login. */}
                        {displayMessage && message !== undefined && (message.type !== "warning" || !isAppInitiatedAction) && (
                            <div
                                className={clsx(
                                    `alert-${message.type}`,
                                    kcClsx("kcAlertClass"),
                                    `pf-m-${message?.type === "error" ? "danger" : message.type}`
                                )}
                            >
                                <div className="pf-c-alert__icon">
                                    {message.type === "success" && <span className={kcClsx("kcFeedbackSuccessIcon")}></span>}
                                    {message.type === "warning" && <span className={kcClsx("kcFeedbackWarningIcon")}></span>}
                                    {message.type === "error" && <span className={kcClsx("kcFeedbackErrorIcon")}></span>}
                                    {message.type === "info" && <span className={kcClsx("kcFeedbackInfoIcon")}></span>}
                                </div>
                                <span
                                    className={kcClsx("kcAlertTitleClass")}
                                    dangerouslySetInnerHTML={{
                                        __html: kcSanitize(message.summary)
                                    }}
                                />
                            </div>
                        )}
                        {children}
                        {auth !== undefined && auth.showTryAnotherWayLink && (
                            <form id="kc-select-try-another-way-form" action={url.loginAction} method="post">
                                <div className={kcClsx("kcFormGroupClass")}>
                                    <input type="hidden" name="tryAnotherWay" value="on" />
                                    <a
                                        href="#"
                                        id="try-another-way"
                                        onClick={() => {
                                            document.forms["kc-select-try-another-way-form" as never].submit();
                                            return false;
                                        }}
                                    >
                                        {msg("doTryAnotherWay")}
                                    </a>
                                </div>
                            </form>
                        )}
                        {socialProvidersNode}
                        {displayInfo && (
                            <div id="kc-info" className={kcClsx("kcSignUpClass")}>
                                <div id="kc-info-wrapper" className={kcClsx("kcInfoAreaWrapperClass")}>
                                    {infoNode}
                                </div>
                            </div>
                        )}
                    </div>
                </div>
            </div>
        </div>
    );
}
```

## 5. Adding a Background Image to the Login Page

You have added an image src/login/assist/image.jpg as a background image for the login page.

Now, modify the CSS to apply this background image.

```
body .kcBodyClass {
    background: url("/login/assist/image.jpg") no-repeat center center;
    background-size: cover;
    background-color: rgb(222, 226, 226);
}
```

Image is located at:

/public/login/assist/image.jpg

## 6. Configure Vite for Asset Loading

To ensure that assets like images load correctly, update the Vite configuration file.

```
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import { keycloakify } from "keycloakify/vite-plugin";

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        react(),
        keycloakify({
            accountThemeImplementation: "none"
        })
    ]
});
```

## 7. Deploying & Viewing the Custom Keycloak Theme

### Step 1: Build the Custom Keycloak Theme

Run the following command to build your theme:

```
npx keycloakify
```

#### Then restart Keycloak and copy the theme into the container:

```
docker cp dist_keycloak/theme/keycloakify keycloak:/opt/keycloak/themes/
docker restart keycloak
```

Your background image should now be applied!

### Step 2: Restart Keycloak to Load the Custom Theme

```
npx keycloakify start-keycloak
```

The ftl files from ./dist_keycloak/theme are mounted in the Keycloak container.

Keycloak Admin console: http://localhost:8080

user:     admin
 
password: admin


Your theme is accessible at:

https://my-theme.keycloakify.dev/

## 8. Verify the Theme in Keycloak

Login to Keycloak Admin Console

Go to Realm Settings → Themes

Select Your Custom Theme from the dropdown

Click Save and refresh the login page

Your customized Keycloak theme is now live!

