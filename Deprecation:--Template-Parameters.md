## Changes to Tpetra's template parameters and offset types

### Proposal:  
The Tpetra Team proposes the following changes to the templated types and 
data types used by Tpetra classes.

* Remove template parameters for LocalOrdinal and GlobalOrdinal.  
  - LocalOrdinal will be determined during configuration (via CMake options)
    to be int32_t or int64_t.  
  - GlobalOrdinal will be fixed to int64_t.  

* Specify the maximum number of nonzeros per MPI rank (sometimes called 
  "offset_type") to be Kokkos::size_type.

* Remove template parameter for Node.  
  - Support for various execution spaces (e.g., OpenMP, CUDA) will be determined
    during configuration via CMake options.  
  - When a single execution space is built, Tpetra operations will be done in
    that execution space by default.  
    E.g., Tpetra::MultiVector<scalar_t>::dot(multiVecA, results);  
  - New capability to perform heterogeneous operations will be enabled by 
    new interfaces that take execution space as a function parameter, allowing
    multiple execution spaces to be used.   
    E.g.,
    Tpetra::MultiVector<scalar_t>::dot(executionSpaceEnum, multiVecA, results);

### Justification:
* Both build times and library size are smaller when fewer template parameters
  are used and instantiated.
* The Tpetra user interface is simpler with fewer template parameters.
* The Tpetra code base is more maintainable with fewer template parameters.
* Adding the Kokkos ExecutionSpace as an option for Tpetra operations enables 
  heterogeneous computation across multiple execution spaces.

### Impact:
* Code changes in applications and other Trilinos packages will likely 
  be simple and straightforward.  Most appplications already use the proposed
  local and global ordinal types.  When applications have used typedefs to 
  represent Tpetra data types, only the typedefs will need to change.
* Less commonly used combinations of LocalOrdinal and GlobalOrdinal will
  no longer be supported.

### Proposed schedule:
* Conversion path completed and documented in Tpetra:  FY18 Q3
* Deprecation warnings for existing templated interface added:  FY18 Q3
* Adoption required; deprecated code removed:  FY19 Q1

### Questions, comments, concerns:
* Please email tpetra-developers@software.sandia.gov or contact a 
  member of the Tpetra team.

Date: January 30, 2018  
SAND2018-1009 O
