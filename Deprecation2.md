## Removal of DynamicProfile option for creation of CrsGraph and CrsMatrix

### Proposal:  
The Tpetra Team proposes removal of the DynamicProfile option for creation of 
CrsGraph and CrsMatrix.  The default graph/matrix construction will use 
the equivalent of the current StaticProfile option, in which users specify 
upper bounds on the number of nonzeros per local matrix or local row.
Nonzero-insertion methods will fail if memory bounds are exceeded.
   
### Justification:
* Dynamic memory allocation in support of DynamicProfile is expensive and,
  in the case of GPUs, infeasible.
* StaticProfile construction is more easily optimized for good performance.
* Support of both StaticProfile and DynamicProfile greatly increases the 
  complexity (statefulness) of Tpetra and decreases its maintainability.

### Impact:
* Applications that currently use DynamicProfile construction will need to 
  switch to StaticProfile.  This switch will involve obtaining counts of
  number of nonzeros per local matrix or local row, and providing them to 
  Tpetra Graph or Matrix constructors.  
* Users can begin this conversion at any time using the current StaticProfile 
  interfaces.  
* Best practice examples will be developed by the Tpetra team.

### Proposed schedule:
* Best practice examples for StaticProfile construction completed:  FY18 Q2
* Conversion path completed and documented in Tpetra:  FY18 Q3
* Deprecation warnings for DynamicProfile construction added:  FY18 Q3
* Adoption required; deprecated code removed:  FY19 Q1

### Questions, comments, concerns:
* Please email tpetra-developers@software.sandia.gov or contact a 
  member of the Tpetra team.

Date: January 30, 2018  
SAND2018-1010 O
