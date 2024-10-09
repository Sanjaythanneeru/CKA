Day 04 documentation

1. **Challenges of using standalone containers**
   - **Resource Management**: Standalone containers can consume significant system resources, leading to potential performance issues if not properly managed. Resource limits need to be set to prevent any single container from overwhelming the host system.
   - **Networking Complexity**: Managing networking configurations can be complicated, especially when multiple containers need to communicate with each other or external services. Configuring port mappings and network drivers can become cumbersome.
   - **Data Persistence**: Containers are ephemeral by nature, meaning data stored within them is lost when they are stopped or removed. Setting up persistent storage solutions requires additional configuration and management.
   - **Isolation and Security**: While containers provide a degree of isolation, they share the host OS kernel, which can lead to security vulnerabilities. Ensuring containers are properly isolated and secure is essential but can be challenging.
   - **Deployment and Scaling**: Scaling applications using standalone containers can be less efficient than using orchestration tools like Kubernetes. Managing multiple containers manually can lead to operational overhead and inconsistencies.
   - **Monitoring and Logging**: Implementing effective monitoring and logging solutions can be difficult. Standalone containers may lack built-in tools for tracking performance metrics, requiring additional setup to collect and analyze logs.
   - **Configuration Management**: Keeping configuration consistent across different environments (development, testing, production) can be tricky. Ensuring that environment variables and configuration files are correctly set up for each container can lead to errors.
   - **Limited Ecosystem Support**: While standalone containers are popular, some advanced features and best practices available in container orchestration platforms may be inaccessible, limiting scalability and robustness.
   - **Dependency Management**: Managing dependencies and ensuring compatibility between containers can become complex, especially when different containers require different versions of libraries or services.
   - **Learning Curve**: For teams new to containerization, there may be a steep learning curve to effectively utilize and manage standalone containers. Understanding best practices and troubleshooting issues can take time and resources.
