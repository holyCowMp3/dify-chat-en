# Project Introduction

**Dify Chat** is an AI Web application based on the Dify API, providing out-of-the-box application configuration management functionality, supporting the operation of different types of Dify applications, and adapting to various rich output forms such as deep thinking, chain of thought, charts, etc.

If you think this project is good, please give it a Star ⭐️～

![Dify Chat](/banner.png)

## Why Choose Dify Chat

If you need to use Dify, but the official Web application doesn't meet your needs, or you need to customize the interface, what would you choose?

- Fork and modify the source code? If you need to upgrade Dify later, code merging during each upgrade will be exhausting
- Fork based on the official template? The features provided in the template are outdated and cannot keep up with the latest Dify version features
- Since both of the above are built on Next.js and not optimized for Turbopack, they consume a lot of memory and have slow hot reload speeds. So you might choose to start from scratch. But to replicate the official Web application features, you may need to spend a lot of time and effort.

Dify Chat is designed around the following key features:

- **Out of the Box**
- **Fully Open Source**
- **Flexible Architecture**
- **Theme Customization**
- **Responsive Design**
- **Rich Content Support**

### Out of the Box

Just one command to complete all installation and startup work, no complex configuration needed. All you need to do is: run the startup command, enter your Dify API Key, and start using it right away.

### Fully Open Source

Dify Chat uses the MIT open source license, and all source code is fully open. You can:

- **Fork Repository**: Fork the original repository on GitHub to get the complete project code
- **Free Modification**: Modify the interface, add new features, or optimize existing logic according to business needs
- **Secondary Development**: Deep customization based on the existing architecture to create your own AI application platform
- **Contribute Code**: Give back improvements to the community through Pull Requests to help the project continue to improve

### Flexible Architecture

Dify Chat uses a frontend-backend separation design, fully considering user experience, scalability, and maintenance costs, divided into two sub-packages:

- React APP, a pure frontend application that provides direct integration with the Dify API. You just need to fill in the application configuration to use it directly
- Platform, a platform application that provides application configuration management, data storage, and Dify API proxy functions, suitable for scenarios with higher security and stability requirements

The two complement each other to form a complete solution from configuration management to user interaction. You can also use only one of them according to your actual needs and integrate with other systems.

The React APP rebuilt from scratch, with millisecond-level hot updates based on Rsbuild, frees you from the limitations of the official Web application's lag and slowness, allowing you to quickly achieve personalized needs.

### Theme Customization

Dify Chat is built on TailwindCSS and Ant Design, providing flexible theme customization capabilities, allowing you to easily adjust the interface style according to brand needs or personal preferences.

### Responsive Design

Dify Chat adopts responsive design principles, supporting desktop, tablet, and mobile device access, ensuring consistent user experience and functionality across different screen sizes.

### Rich Content Support

Dify Chat supports rendering multiple content types, making AI responses more vivid and practical:

**Image and Video Rendering**: Supports rendering image and video content. When expressing content, rich images and dynamic videos are often more impactful than text, especially suitable for tutorials, demonstrations, and other scenarios.

**Code Highlighting**: Built-in code syntax highlighting functionality, supporting multiple programming languages, making code display clearer and more readable, improving the developer experience.

**Chart Visualization**: Supports various types of chart rendering, such as Mermaid, Echarts, etc., helping users better understand and analyze data.

**Interactive Elements**: Supports interactive components such as forms, allowing users to perform operations directly in the conversation interface, improving interaction efficiency.

Rich content support allows you to arrange content in Dify applications in any way, expressing information in the most suitable way. Whether it's technical documentation, data analysis reports, or creative content creation, you can get the best display results.

## Tech Stack

- React v19
- Next.js v15
- Prisma ORM v6
- Ant Design v5
- Ant Design X v1
- Rsbuild v1
- Tailwind CSS v3
- TypeScript v5
- Pnpm v10
