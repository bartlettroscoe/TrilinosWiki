User and client support is an essential element of any actively used and developed software product.  Any useful software product requires corrective and perfective maintenance, especially as its ecosystem (hardware environments, compilers, etc.) evolves.  Support plans and planning activities are also essential, especially for clients.  

The Trilinos Project is committed to providing support for its user community, prioritizing support activities according to its mission requirements.  This policy sketch provides the framework for conducting support activities, roles and responsibilities.  Detailed support strategies for funded projects should be consistent with the policy stated here and provide further detail.

## Trilinos Support Components

The Trilinos Project support strategy is composed of three components:
- **Prevention:** The most effective approach to providing support is to produce high-quality software design, implementation, documentation and testing.   This enables clients and users to more easily work with the product and reduces the number of support events.  For an ongoing project, the biggest benefit comes from eliminating regressions as the software environment changes.  Careful testing is essential.
- **Diagnosis:** When support is required, it often requires diagnosing the cause of the support issue.  Trilinos contains a large and complex product suite, so assistance from Trilinos leadership can be essential in identifying the scope and responsibility for handling a support event.
- **Resolution:** Many support events are easily fixed after the event source is determined.  Trilinos team members are committed to resolving support events as priorities dictate.  Some events are very challenging to resolve and may require a long, concerted effort.  This is especially true of issues that arise only during high-end simulations.  For these events, Trilinos leadership is committed to assigning resources as priorities and urgencies require.

## Trilinos Support Philosophy

Software support is a significant cost.  Many studies estimate support and maintenance to be 70 - 80% of the total software project cost.  Any improvement in support will have significant impact on quality, cost and time.

Support of actively developed software is most effective when those who are involved in the development effort take ownership of support planning, and are the first point of contact with issues arise.  These people have the fullest awareness of development goals, target computing environments and project priorities.  Furthermore, these people should invest adequate time and resources into testing and documentation infrastructure, preparing for the future when the code is no longer actively developed but still used, and support is transferred to others.

Support of software that is not actively developed starts with the Trilinos framework team.  This team will work with clients and users to determine the best approach for acquiring technical and funding resources.  
 
## Trilinos Support Policy

### General Trilinos User Community 

The Trilinos Project considers its interactions with the general user community to be an essential element of our success.  This user community, numbering in the tens of thousands, provides essential value and vibrancy to the project.  Contributions include fault detection and correction, ideas for improvement and new capabilities and interactions that spur creativity toward next-generation capabilities.  We will support the general community as we have resources, and to the extent that requests are consistent with funded client and user needs.

### Funded Projects

Most Trilinos capabilities are developed within projects targeted at specific modeling and simulation capabilities.  Direct funding is provided for both application developers and for Trilinos developers and support staff.  For these projects (called **funded projects** below), the following statements apply:
1. _**The Trilinos team is committed to improving the quality of its products, especially toward the goal of improving the client and user experience:**_ We welcome any input from our clients and users that can help us improve our product and support capabilities.
1. _**<trilinos-help@software.sandia.gov> - Funded project clients and users can submit any support issue to the Trilinos Project Help email address:**_ Diagnosing the cause of a support event can be very challenging.  In order to assist clients and users, the Trilinos Project provides a single point of contact for any funded project: <trilinos-help@software.sandia.gov>.  Messages sent to this address will be handled promptly in order to diagnose the source and establish a resolution strategy.  If the client or user does not need diagnosis assistance, they are encouraged to work directly with the Trilinos issue tracking system.
1. _**The Trilinos Project will work with clients and users to resolve support events as quickly and efficiently as possible:**_ Using our support processes, the Trilinos team will resolve issues in collaboration with our clients are users.  For large funded projects we will conduct regular meetings to review and resolve issues.
1. _**Trilinos support will be an explicitly planned and funded activity within the funded project:**_ While the Trilinos project has dedicated support staff, staffing and funding for support must come from aggregate resources provided by funded projects.  Specific project budget amounts should be set aside for support. This is especially true for package that are in use but not funded for development (called Production Maintenance in the Trilinos Lifecycle Model).
1. _**Trilinos Framework staff with work with funded projects to develop a support plan that is consistent with Trilinos support policies:**_ The Trilinos support policy provides a framework for support planning and activities, but should be customized and fully developed as part of the funded project planning.

### Notes:
1. _**Policy Scope:**_ This policy formally applies to any _Production Growth_ package development.  It also applies to modifications of other Trilinos packages that are not being actively developed but are essential and may require modification. It is encouraged for packages in other phases (_Research Stable_ or _Production Maintenance_).
1. _**Exceptions:**_ Yes, there will be exceptional circumstances where you need to bypass the support policy, but it should be the _exception_ and addressed as soon as possible.  Exceptions should be documented and reviewed by Trilinos framework staff.
1. _**Trilinos framework team role:**_ Trilinos framework team members are responsible for support policy management, tools and testing environments. They will work with funded project teams to assure that funded project support is effective and consistent with Trilinos policies.  Trilinos framework team members will provide funded projects with support in the use of build, integration and testing infrastructures, understanding Trilinos issue management, testing and other policies.  If framework staffing demands are significant, the funded project should budget for this effort.
1. _**Related policies:**_ In order to improve future support capabilities and reduce support costs, Trilinos developers are expected to follow Trilinos policies for [Issue Management](https://github.com/trilinos/Trilinos/wiki/Managing-Trilinos-Project-Issues), [Testing](https://github.com/trilinos/Trilinos/wiki/Trilinos-Testing-Policy) and [Pre-push Testing](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing).

### Questions?
Contact Mike Heroux <maherou@sandia.gov>