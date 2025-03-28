# Keycloak

## Keycloakify Overview

Keycloakify is a tool for creating custom Keycloak themes, enabling you to modify the appearance and behavior of Keycloak's user interfaces. This includes:

- **Login Theme**: The UI for login and registration pages, displayed to users when they attempt to log in or sign up.
- **Account Theme**: The account management interface, where users can update their email, change their password, and manage other account settings.
- **Email Theme**: The templates used by Keycloak for automated emails, such as email confirmation or password reset notifications.
- **Admin Theme**: The Admin Console interface, used by administrators to configure Keycloak.

For a visual preview of these UIs as they appear with Keycloak's built-in theme, visit the **Storybook demonstration**.

## Why Choose Keycloakify?

You might be wondering why you would need a third-party tool like Keycloakify to create your custom UIs instead of relying solely on Keycloak's built-in theming system. Here are a few reasons:

- **Leverage Modern Frontend Technologies**: Keycloakify enables you to use TypeScript, React, Angular, Svelte, and any styling solution or component library you prefer, such as Tailwind, MUI, shadcn/ui, or plain CSS.
- **Streamlined Testing**: Keycloakify makes it easy to test your theme both inside and outside Keycloak, with hot reloading for a smoother development experience.
- **Automated Theme Bundling**: Keycloakify bundles your theme into a JAR file, ready to import directly into Keycloak.
- **Version Compatibility**: Themes generated with Keycloakify are backward compatible with Keycloak versions as far back as 11 and are designed to remain compatible with future Keycloak updates.
- **Built-In Real-Time Validation**: Keycloakify includes real-time frontend validation by default. For example, users receive instant feedback, such as *"The password must be at least 12 characters long,"* rather than waiting until they press the submit button.
- **Community Support**: If you're stuck or need guidance, reach out through our **Discord channel** or **GitHub issues**.

If you’re still unsure or want a better understanding before committing to using Keycloakify, check out this guide:

[How does Keycloakify work?](#)

## Pick Your Framework: React, Angular, or Svelte

Keycloakify supports **React**, **Angular**, and **Svelte**, allowing you to work with the framework you're most familiar with. 

- If you're only making **CSS-level customizations**, any of these frameworks will work.
- **React is recommended** for the smoothest experience, as it has the most complete integration.

### Considerations for Angular and Svelte Users:
- **Account Themes**: The starting UI differs from Keycloak's default, requiring additional adjustments.
- **Admin Themes**: Only **React** supports custom Admin UIs.
- **Angular Setup**: Keycloakify **cannot be directly installed** in an existing Angular project. Themes must be standalone projects or a subproject in a monorepo.

React provides the most seamless experience, but Angular and Svelte are fully supported for login and account themes with some extra effort.

## Quick Start

### Clone the Starter Template

```sh
git clone https://github.com/keycloakify/keycloakify-starter
```

# CSS Customization

This page is a must-read. Even if you plan to redesign the pages at the component level, you should at least understand how to remove the default CSS styles.

## Understanding the CSS Class System

When you inspect the DOM in Storybook, you’ll notice most elements have at least a couple of classes applied to them:

- A class starting with **kc**, for example `kcLabelClass`.
- One or more classes starting with **pf-**, for example `pf-c-form__label`, `pf-c-form__label-text`.

### Inspecting an Input Label on the Login Page

- Classes beginning with **kc** don’t have any styles applied to them by default. Their sole purpose is to serve as selectors for your custom styles.
- Classes beginning with **pf-** are **Patternfly** classes. **Patternfly** is a CSS framework created by RedHat, similar to Bootstrap, that the Keycloak team uses to build all of its UIs.

### Removing Default Patternfly Styles

What you’ll want to do is partially or completely remove the **Patternfly** styles and then apply your custom ones.

## Applying Your Custom CSS

**Do not edit** any file in the `public/keycloakify-dev-resources` directory. These files are used by **Storybook** to simulate a Keycloak environment during development, and they aren't part of your actual theme.

To apply your custom CSS style, use the **kc** classes to target the components.

### `src/login/main.css`
```css
.kcLabelClass {
   border: 3px solid red;
}


