### Tpetra team FY18 planning
DELIVERY:  FY18 Q1; POC: Devine
Identification of performance, maintainability, quality issues; feasibility analysis; priority setting
### Sparse Matrix-Matrix multiplication performance 
DELIVERY:  FY18 Q2; POC: Siefert
Integration of KokkosKernels SpGEMM on GPUs; performance evaluation
MPI+X improvements, including algorithmic improvements ported from Epetra 
### Benchmark apps; platform/configuration set-up
DELIVERY:  FY18 Q2 -- identification, repository, configure, build; FY18 Q4 -- automated testing; POC: Hu
MueLu setup – use MueLu driver
CG – use benchmark in Tpetra 
Assembly -- best practice code
Import / Export
Overlap – Ifpack2 Additive-Schwarz
BlockCRS
### Extended test coverage beyond Trilinos’ regular testing
DELIVERY: FY18 Q2;  POC:  Trott
### Evaluate potential gains from reducing global reductions; determine plan and priority for action
DELIVERY:  Evaluation in FY18 Q2; POC: Luchini
Benchmark MueLu vs ML set-up; identify potential benefit and semantic changes
### Ensure and evaluate usage of non-UVM memory for MPI communication routines
DELIVERY:  FY18 Q3; POC:  Hoemmen
### Map / Graph / Matrix / Multivector design and performance improvements
DELIVERY:  FY18 Q3 for development and deprecation; FY19 Q1 for removal of deprecated code; POC: Trott
Includes performance improvements for Matrix / Graph construction and fill
Includes Deprecation of Dynamic Profile (insert operation that can fail, examples of new application usage, utilities for reallocation, support for knowing max # entries but not max # entries per row)
Includes reducing unsafe data access by returning of unmanaged Kokkos::View
### Reduce number of template parameters to speed build times and reduce library size
DELIVERY:  FY18 Q3 for development and deprecation; FY19 Q1 for removal of deprecated code; POC: Hoemmen
Fix GlobalOrdinal to be int64_t
Allow compile-time option for LocalOrdinal:  int32_t or int64_t
Determine node type from compile-time option rather than template parameter
Includes defining offset_type to be Kokkos::size_type
### Best practices documentation, User's guide
DELIVERY:  FY18 Q4; POC:  Fuller
