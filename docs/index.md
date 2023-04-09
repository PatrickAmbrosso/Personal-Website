# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

```yml title="mkdocs.yml" 
mkdocs.yml    # The configuration file.
docs/
    index.md  # The documentation homepage.
    ...       # Other markdown pages, images and other files.
```
<figure markdown>
  ![Image title](https://dummyimage.com/600x400/){ width="300" }
  <figcaption>Image caption</figcaption>
</figure>

[Mail Me](mailto:patrickambrosericky@gmail.com){ .md-button }



=== "C"

    ``` c title="mainfunc.c"
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++ title="mainfunc.cpp"
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```

??? note

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.


$$
\operatorname{ker} f=\{g\in G:f(g)=e_{H}\}{\mbox{.}}
$$

The HTML specification is maintained by the W3C. The FQDN of the website is very important.



## Why Containers?
Containers are the talk of the town these days. It is no secret that in the recent years a lot of businesses have started to adopt containerization technology and are starting to reap the benefits out of it. To understand why containerization came to existence, it is required to understand the problems that it aimed to fix in the industry.

### Age of Bare-Metal Hardware
Applications and services form the basis of all enterprises and businesses. These applications and services run on servers. And by convention, due to several operational best practices, one server runs only one application. This meant that for `n` number of applications, `n` number of servers were required to be bought, licensed for, set-up, hosted, maintained, patched and decommissioned by the IT operations team.

This might not sound like a tedious and an error prone task, but the following list captures some of the shortcomings of this method.
1. **Capacity Calculations** - Miscalculation (underestimation/overestimation) of the capacity needed to host and serve the application.
2. **Untapped Potential** - Closely related to the capacity calculation, if the required capacity is overestimated, the hardware and the capacity of the server goes underutilized.
3. **Service Constraints** - The opposite end of the spectrum of untapped potential is being bottlenecked by the hardware due to insufficient space for the application to grow. To circumvent this, IT bought big and bigger hardware, which circles back to the resource being underutilized.
4. **Licensing Cost** - Often operating systems like those that run enterprise grade applications cost a lot of money to buy and maintain.
5. **Patch Work** - Patching the operating systems and application means that the service had to be taken down completely and worked on.

### Virtualization to the rescue

#### VMs, the first step in virtualization
At the time when the operations team were so frustrated about the single-host, bare-metal hardware architecture, [VMware](https://www.vmware.com/) came up with the solution called as Virtual Machines. Virtual Machines (VMs) are ways to run multiple, isolated operating systems in a single operating system. This shook the industry overnight (for the better of course). The following are some of the advantages when it comes to VMs over bare-metal hardware.
- Each machine (server) could now host as many application as the server's resources allow.
- This reduced the need to procure new servers for each of the new application that the business decides to implement.
- The environments in which these applications reside (called the virtual machine) came with its own operating system, meaning full isolation and data security.
- This enabled companies to fully utilize their existing infrastructure.

However, VMs also had some issues and the following are some of the shortcomings.

1. **Wasted Space and Potential** - As each VM comes with its own Operating System, they tend to take up space that could be used by the application itself.
2. **Licensing Cost** - As each VM has to come with its own Operating System, it means that each of those operating systems cost money to buy and patch.
3. **Operational Constraints** -  VMs are less portable and are slow to boot. This was a nightmare for teams that manage a hybrid deployment between cloud and on-premises infrastructure.

#### The Good, the Better and Containers
Containers came into existence to fix the problems that VMs had. In a way Containers are like VMs, in that they provide isolation between the applications and the host hardware and operating system. But, they solve some of the crucial pain points of the previous 2 computing paradigms namely bare-metal and virtual machines, which is tabulated below.

| The Pain Point                                | Containers and Answers                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Clunky OS and Dependencies                    | Containers share the the same OS as the Host Hardware. There is no duplication of the OS for each and every instance of an application being deployed in containers. This also means that there is no additional cost and overhead of maintaining the OS Licenses for each and every instance (bare-metal or VM).                                                              |
| Capacity Miscalculations and Utilization Gaps | Containers utilize only the resources allocated to them. All remaining resources are available for utilization by other containers. This ensures that no resource goes wasted in capacity.                                                                                                                                                                                     |
| Portability and Scaling                       | Being lightweight and conforming to a standard (usually follow a set standard), containers are extremely portable, where almost all cloud providers allow running containers with little to no headway in setting up the environment. Moreover, as containers are units of operations, multiple containers can be deployed thus horizontally scaling the operational capacity. |
| Operational Speed                             | As containers share the kernel with the operating system, they are much faster than VMs to boot-up and get to an operational state.                                                                                                                                                                                                                                                                                                                                                                                |
