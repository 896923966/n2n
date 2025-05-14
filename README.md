## Manual Compilation Instruction Document

## 1. Document Overview
This is an instruction document on manual compilation. It mainly covers the steps for compiling from source code under the Linux system, and also mentions relevant information about Windows and MacOS systems, version-selection suggestions, and thanks for user feedback.
## 2. Compilation Steps in the Linux System
In the Linux system, there is a relatively standard process for compiling software from source code.
(1) Execute ./autogen.sh
First, the ./autogen.sh script is executed. Its function is to automatically generate the files required for configuration, and it helps to detect the system environment, such as checking whether necessary tools and libraries are installed in the system, thus preparing for the subsequent compilation-configuration process.
(2) Run ./configure
Next, the ./configure command is run. This command further, based on the actual situation of the system, carefully checks whether various dependencies meet the compilation requirements. At the same time, it allows users to set some compilation parameters, such as specifying the installation path, enabling or disabling certain functional modules, etc., so as to generate a suitable Makefile and lay the foundation for the actual compilation process.
(3) Execute make
Subsequently, the make command is executed. It compiles the source code according to the rules defined in the Makefile, converting the source code into target products such as executable files or library files.
(4) Optional Step make install
And make install is an optional step. When this command is executed, the compiled files will be installed in the pre-set directories of the system, so that users can conveniently call these installed software or tools from other parts of the system.
## 3. Related Information about Windows and MacOS Systems
For Windows and MacOS systems, due to the differences in their operating-system architectures and environments compared with Linux, and when it comes to optimizing the compilation process or general build options, relying solely on the above-mentioned compilation steps for the Linux system is insufficient. At this time, users need to refer to the dedicated build documentation, which will detail the specific compilation and running methods for these systems, including the required tools, environmental configurations, etc.
## 4. Version Selection Suggestions
In terms of version selection, the document strongly recommends using the latest stable version. This is because the stable version has undergone extensive testing and verification, ensuring better stability and compatibility, and reducing the likelihood of errors and problems during use.
The current dev branch is mainly used for developing and testing the latest, yet-to-be-fully-stabilized features. This branch generally cannot guarantee backward compatibility with the latest stable version and previous dev states. That is to say, the code obtained from the dev branch may not run properly in an environment built based on previous versions.
However, if users are brave enough to try out the latest features and are willing to take the risk of potential instability, the document also encourages users to start compiling from the dev branch. It should be noted, though, that since the code in the dev branch is updated frequently, users need to closely track the rapid changes in the code and promptly understand and adapt to these changes to ensure a smooth compilation process.
## 5. User Feedback
Finally, the document expresses gratitude to users who provide feedback in the Issues section. User feedback is crucial for the continuous improvement and optimization of the project. By collecting and analyzing feedback information such as the problems encountered and suggestions put forward by users during use, the project development team can better identify the deficiencies in the software, promptly fix bugs, optimize functions, and thus continuously enhance the quality and user experience of the software.
<!-- by 谢思源 -->