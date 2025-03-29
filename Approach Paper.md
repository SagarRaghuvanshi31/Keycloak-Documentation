# Approach Paper 

## Customizing Keycloak Login Page

### Table of Contents

#### Objective

#### Proposed Solutions

#### Approach 1: Keycloakify with Docker
3.1. Architecture Diagram
3.2. Description
3.3. Pre-requisites

3.3.1. Hardware Requirements

3.3.2. Software Requirements

3.3.3. Networking Requirements

#### Approach 2: Custom Keycloak Theme with Static Files
4.1. Architecture Diagram
4.2. Description
4.3. Pre-requisites

4.3.1. Hardware Requirements

4.3.2. Software Requirements

4.3.3. Networking Requirements

#### Chosen Approach

## 1. Objective

The objective of this project is to customize the Keycloak login page to provide a better user experience.

We will implement a custom UI theme, add a background image, and ensure seamless deployment using Docker.

Additionally, we will use Storybook to test UI components efficiently.

## 2. Proposed Solutions

Two approaches can be considered for customizing the Keycloak login page:

## Approach 1: Using Keycloakify with Docker to build a React-based Keycloak theme.

## Approach 2: Modifying the Keycloak theme manually by updating static HTML, CSS, and FreeMarker templates.

## 3. Approach 1: Keycloakify with Docker

### 3.2. Description

This approach uses Keycloakify, a tool that allows building React-based themes for Keycloak.

We will create a custom login page, integrate Storybook for UI testing, and deploy the theme using Docker.

#### Pros:

React-based component system for easier UI modifications.

Storybook integration for UI testing.

Can be deployed easily with Docker & Keycloak.

Future updates are manageable with React components.

#### Cons:

Slightly complex setup compared to static themes.

Requires knowledge of React & Keycloakify.

### 3.3. Pre-requisites

3.3.1. Hardware Requirements

Minimum 4GB RAM

2-core CPU

10GB Disk Space

### 3.3.2. Software Requirements

Operating System: Linux / Windows / macOS

Keycloak: v20.0+

Docker: Latest Version

Node.js: v22.9.0

NPM: 8.0+

Storybook for UI Testing

### 3.3.3. Networking Requirements

Open port 8080 for Keycloak

Internet access to download dependencies

## 4. Approach 2: Custom Keycloak Theme with Static Files

In this approach, we manually edit Keycloak’s FreeMarker templates and CSS files to customize the login page.

We directly modify themes/base/login/login.ftl, update the CSS styles, and add a background image.

#### Pros:

ightweight, no extra dependencies.

Faster rendering since it's static.

No need to learn React or install additional tools.

#### Cons:

Hard to maintain & update.

No component-based UI development.

Limited customization options.

### 4.3. Pre-requisites

#### 4.3.1. Hardware Requirements

Minimum 2GB RAM

2-core CPU

5GB Disk Space

#### 4.3.2. Software Requirements

Operating System: Linux / Windows / macOS

Keycloak: v20.0+

Text Editor: VS Code / Nano

FreeMarker Templating Engine

#### 4.3.3. Networking Requirements

Open port 8080 for Keycloak

## 5. Chosen Approach: Approach 1 (Keycloakify with Docker)
   
We have chosen Approach 1 (Keycloakify with Docker) because:

Scalability: Keycloakify allows easy UI updates using React components.

Maintainability: Easier to manage UI changes over time.

Testing: Storybook integration provides a reliable way to test the UI.

Deployment: Docker makes it easy to deploy the theme across multiple environments.
