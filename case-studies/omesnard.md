*Authors: Olivier Mesnard, Lorena A. Barba*

##### Introduction
*Please answer these introductory questions for your case study in a few sentences.*

1) Who are you and what is your research field? Include your name, affiliation, discipline, and the background or context of your overall research that is necessary specifically to introduce your specific case study.

We are members of a computational research group led by Prof. [Lorena Barba](http://lorenabarba.com) at the George Washington University in the department of Mechanical and Aerospace Engineering. 
One of our research projects focuses on studying animal flight by means of unsteady simulations at low/moderate Reynolds numbers, using immersed-boundary methods.
The work we describe in this case study was executed by the first author, PhD student Olivier Mesnard, but it fits in a broader program of research involving other group members.
We do our best to accomplish reproducible research and have for years worked to improve our practices to achieve this goal.
According to the "Reproducibility PI Manifesto," pledged by Barba in 2012, all research code written in the group is under version control and open source, our data is open, and we publish open pre-prints of all our publications.
For the main results in a paper, we prepare file bundles with input and output data, plotting scripts and figure, and deposit them in the [figshare](https://figshare.com/authors/Lorena_A_Barba/97553) repository.
These conditions all apply to our previously published work in Krishnan et al. (2014), studying the aerodynamics of flying snakes.
This case study describes what happened when we set out to not just reproduce our previous results in Krishnan et al. (2014), but complete a full replication using different computational fluid dynamics (CFD) codes: a new code developed in our group, an open-source code developed by another group, and an open-source CFD library.


2) Define what the term "reproducibility" means to you generally and/or in the particular context of your case study.

The starting point for our understanding of reproducibility is contained in the pledge "Reproducibility PI Manifesto" (Barba, 2012) which includes these steps:

1. teaching group members about reproducibility;
2. maintaining all code and writing under version-control;
3. carrying out verification and validation and publishing the results;
4. for main results in a publication, sharing data, plotting scripts, and figures under CC-BY; 
5. uploading preprints to arXiv at the time of submission of a paper;
6. releasing code no later than the time of submission of a paper;
7. adding a "Reproducibility" statement to each publication;
8. keeping an up-to-date web presence.

Some of these items have to do with making our research materials and methods open access and discoverable. 
The core of this pledge is releasing the code, the data, and the analysis/visualization scripts. 
Already this can be time consuming and demanding.
Yet, we have come to consider these steps the most basic level of reproducible research.
On undertaking a full replication study of a previous publication by our research group, we came to realize how much more rigor is required to achieve this, in the context of computational fluid dynamics of unsteady flows.
We use the term "full replication" in the sense presented by Peng (2011), that is, completing an independent study using new methods to collect new data, arriving in the end at the same scientific findings.
In computational fluid dynamics, full replication of the findings can involve using a different code that implements the same numerical method, or a code that implements a different numerical method altogether but solves the same mathematical model.
Because we are solving the Navier-Stokes equations—an unsteady and nonlinear model—certain problem scenarios can present particular challenges to replication.

##### Workflow diagram

The core of your case study is a diagrammatic workflow sketch, which depicts your the entire pipeline of your workflow. Think of this like a circuit diagram: boxes represent steps, tools, or other disjoint components, and arrows show how outputs of particular steps feed as inputs to others. This diagram will be complemented by a textual narrative.

We recommend the site draw.io for creating this diagram, or you are also welcome to sketch this by hand. While creating your diagram, be sure to consider:

* specialized tools and where they enter your workflow
* the "state" of the data at each stage
* collaborators
* version control repos
* products produced at various stages (graphics, summaries, etc)
* databases
* whitepapers
* customers

Each of the two example case studies include a workflow diagram you can also use for guidance.

Please save your diagram alongside this completed case study template.

[workflow diagram](omesnard.pdf)

##### Workflow narrative

Referring to your diagram, describe your workflow for this specific project, from soup to nuts. Imagine walking a friend or a colleague through the basic steps, paying particular attention to links between steps. Don't forget to include "messy parts", loops, aborted efforts, and failures.

It may be helpful to consider the following questions, where interesting, applicable, and non-obvious from context. For each part of your workflow:

* **Frequency:** How often does the step happen and how long does it take?
* **Who:** Which members of your team participate (or not)?
* **Manual/Automated:** Is the step automated or does it involve human intervention (if so, is it recorded)?
* **Tools:** Which software or online tools are used in the step? How are they used?

In addition to detailing the steps of the workflow, you may wish to consider the following questions about the workflow as a whole:

* **Data:** Is your raw data online?
   * Is it citeable?
   * Does the license allow external researchers to publish a replication/confirmation of your published work?
* **Software:** Is the software online?
   * Is there documentation?
   * Are there tests?
   * Are there example input files alongside the code?
* **Processing:** Is your data processing workflow online?
   * Are the scripts documented?
   * Would an external researcher know what order to run them in?
   * Would they know what parameters to use?

*(500-800 words)*

Our research lab has developed over the years a consistent workflow that, we believe, leads to reproducible research.
A previous study coming out of our lab, published in Krishnan et al. (2014), already satisfies the criteria of the "Reproducibility PI Manifesto" (Barba, 2012). 
[cuIBM](https://github.com/barbagroup/cuIBM), the code used for that study, is version-controlled and open source (hosted on GitHub); we completed a [Validation & Verification study](http://dx.doi.org/10.6084/m9.figshare.92789) that is published openly on figshare; the data and main figures of the paper are also available on figshare under CC-BY, and the paper [pre-print](http://arxiv.org/abs/1309.2969) is available on arXiv (revised post peer review).
Here, we describe our effort to achieve full replication of our previous conclusions in Krishnan et al. (2014) about the aerodynamic characteristics of the flying snakes' body cross-section.
We used a total of four CFD codes to reproduce and replicate our own findings in a scenario of highly unsteady flow, dominated by vorticity. In every case, there were failures and difficulties, leading to improvements in our workflow and conclusions about the special challenges for reproducibility in our application scenario.

Krishnan et al. (2014) studies the aerodynamics of flying snakes using an in-house solver for the incompressible Navier-Stokes equations.
The solver applies an immersed-boundary projection method (Taira and Colonius, 2007) that requires the solution of a modified Poisson system on a structured Cartesian grid. 
It uses an iterative algorithm provided by an external linear-algebra library: the [CUSP](https://github.com/cusplibrary/cusplibrary) library to solve a linear system on a single GPU.

The first code we used to attempt replication is a component of the well-known open-source CFD library, [OpenFOAM](http://www.openfoam.org/).
IcoFOAM is the incompressible laminar solver, applying the finite-volume method on an unstructured body-conforming mesh.
We chose IcoFOAM because it is widely used, open-source, and documented—both code documentation and a users' guide are available.
After a period of several weeks required to learn to use the software, the first batch of simulations with IcoFOAM were a failure.
With unstructured-mesh finite-volume solvers, the mesh generation step is most often what determines the quality of the solution, and some meshes would result in unphysical results.
Visualizations of the vorticity field often showed spurious spots of spontaneously generated vorticity at random locations, due to "bad triangles."
The problem only disappeared when we replaced the mesh generation method altogether: we moved from `GMSH` to `snappyHexMesh`.
Visualizations of the pressure revealed in some cases a wave of back-pressure coming from the downstream domain boundary.
This affected the measured quantity (lift coefficient of the cross section) in just _some_ cases, while in others we matched our previous results.
After going back to documentation, published examples, and various user discussion forums, we tried several ways to modify the boundary condition (BC) at the outflow, but none could fix this problem.
In the end, a fortuitous look at the error message when boundary conditions are specified wrong revealed one option, _advective BC_, which is just what we needed.
This option was absent from the volumes of documentation, examples and user posts we had studied.
And on looking for an option like it, we had searched with the term _convective BC_, a synonym that we're more used to.
Overall, it would be a full year of failed yet persistent efforts before simulations with IcoFOAM replicated the findings of Krishnan et al. (2014) in terms of the lift characteristics of the snake cross-section. 

In the process of working with IcoFOAM, we improved and developed new diligent reproducibility practices. The "top three" of these practices are: automating all post-processing steps with ParaView to use in batch mode and avoid interaction with the GUI; storing the command-line input in scripts for mesh generation and running of the solver; recording the log files and extracting data from them to update the (digital) lab notebook.

We then moved on to [IBAMR](https://github.com/ibamr/ibamr), an open-source software by [Boyce Griffith](http://www.cims.nyu.edu/~griffith/), hosted on GitHub. The aim was to reproduce our previous results with another immersed-boundary (in contrast to body-conforming) solver, but one that uses a different formulation. 
IBAMR implements an immersed-boundary method on a Cartesian grid, allowing adaptive mesh refinement by means of the [SAMRAI](https://computation.llnl.gov/project/SAMRAI) library, and solving the linear systems of equations by means of the [PETSc](http://www.mcs.anl.gov/petsc) library.
Bhalla et al. (2013) published a detailed validation of the software, while various examples are included in the code repository.
IBAMR is written as a library that requires the user to write a driver program to call it.
We also needed to write post-processing scripts for the output of IBAMR, in this case using VisIt. 
After a few weeks of this preparation, we ran a batch of simulations for varying values of the Reynolds number and angle of attack (as before). 
They all failed at first to reproduce the published results in Krishnan et al. (2014).
In the end, we were able to match the results in almost all cases by running IBAMR imposing "no slip" in all mesh points lying _inside_ the body (not just the boundary).
This is not an intuitive option, since all immersed boundary methods work by imposing no slip at the boundary nodes.
Although publications using IBAMR report on using this scheme of imposing a constraint on interior mesh points, no discussion is offered on why this is necessary.
One case continues to give slightly different results: the case with Reynolds number 2000 and angle of attack 35 degrees. 
This is the key case of the batch, which in the original work exhibits an enhanced lift coefficient: the crux of our previous study.
Although we still obtain lift enhancement with IBAMR, the effect is less pronounced.
We can say that the _scientific findings_ of Krishnan et al. (2014) have been fully replicated, but we do see noticeable differences in the details of the flow characteristics.

In view of the experience described above, we cannot help but speculate that if researchers were using either IcoFOAM or IBAMR in an original study (instead of attempting a full replication of a previous study), they may not have continued as far as we did, and could have ended up publishing wrong results.
The characteristic feature of the flying-snake cross-section is that it exhibits lift enhancement at 35 degrees angle of attack.
This precise feature was inhibited by the challenges with boundary conditions in simulations of unsteady vortical flows.
It simply does not appear if the simulations are not very carefully set up.
The difficulty of reproducibility here comes from the application scenario: unsteady, vortex-dominated flows are just very hard to compute correctly.

The [cuIBM](https://github.com/barbagroup/cuIBM) and [PetIBM](https://github.com/barbagroup/PetIBM) codes are both being developed in our research lab and implement the same immersed-boundary method (Taira and Colonius, 2007).
The GitHub code repositories include code documentation with [Doxygen](www.doxygen.org), users' documentation (on the GitHub wiki), as well as basic examples/tutorials.
cuIBM uses [CUSP](https://github.com/cusplibrary/cusplibrary), an open-source library for sparse linear algebra on CUDA-architecture GPUs. 
We used cuIBM again to confirm the reproducibility of the published findings in Krishnan et al. (2014)
The input parameters are available in the code repository while reproducibility packages can be downloaded from figshare. 
It's important to remark that we had to use the _same version_ of the code, with the _same version_ of the linear-algebra library to re-obtain the same numeric answers.
In fact, our first attempts used a newer version of the CUSP library, and failed to replicate the findings!
In PetIBM, we implemented the same immersed-boundary method, but using the [PETSc](http://www.mcs.anl.gov/petsc/) library to solve the modified Poisson system on a distributed-memory machine. 
Even though the mathematical formulation in cuIBM and PetIBM is exactly the same, we observed that a different linear-algebra library could change the results.
As of this writing, we have been unable to replicate the findings of Krishnan et al. (2014) with PetIBM—again, despite the fact that this code use the _same_ numerical scheme as cuIBM, was written by the same researchers and both codes have been verified and validated on the same benchmarks.
The only algorithmic difference between the two is contained in the linear solvers (provided by libraries).
In addition, the two codes run on different hardware, exploiting different parallelism modes (many-core on one hand, distributed message-passing on the other).

The lessons learned from this case study are sobering.
First, the vigilant practice of reproducible research must go beyond the open sharing of data and code:
automation of every pre- and post-processing step and digital logging of all actions are also needed.
Second, certain application scenarios pose special challenges.
Here, we are working with the Navier-Stokes equations applied to highly unsteady flows dominated by vorticity, a particularly tough application for reproducibility.
Third, extra care is needed when using external libraries for iterative solution of linear systems.
Even if everything else is reproduced, the linear-algebra library may introduce uncertainties.

Our reproducibility practices have tightened up substantially after this project.
We now use Python to automate every pre- and post-processing step.
All scripts are version-controlled, the code is documented and allows command-line arguments (to avoid code modification from users). 
We also take advantage of the Python interpreter included in the visualization tools [Paraview](http://www.paraview.org/) and [VisIt](https://wci.llnl.gov/simulation/computer-codes/visit/) to automate the post-processing part of the numerical solution from OpenFOAM and IBAMR. 
In addition to that, we use Jupyter Notebooks and Markdown files to present project advances during regular meetings—and our group-meeting notes are version-controlled and hosted on GitHub.

As we now prepare a manuscript to publish the results of this project, it is being written using LaTeX and is version-controlled in its own GitHub repository to facilitate collaboration between authors. 
To advocate open-science, the manuscript will be first available on arXiv. We will also provide a reproducibility package for all simulations and figures reported in the manuscript. 
These packages include the version of the software (as well as that of its dependencies), the input parameters (for both the simulation and the post-processing), information related to machine architecture, and the necessary scripts to run and post-process the simulation.



*References*:
- Bhalla, A. P. S., Bale, R., Griffith, B. E., & Patankar, N. A. (2013). A unified mathematical framework and an adaptive numerical method for fluid–structure interaction with rigid, deforming, and elastic bodies. Journal of Computational Physics, 250, 446-476.
- Krishnan, A., Socha, J. J., Vlachos, P. P., & Barba, L. A. (2014). Lift and wakes of flying snakes. Physics of Fluids, 26(3), 031901.
- Taira, K., & Colonius, T. (2007). The immersed boundary method: a projection approach. Journal of Computational Physics, 225(2), 2118-2137.

##### Pain points
*Describe in detail the steps of a reproducible workflow which you consider to be particularly painful. How do you handle these? How do you avoid them? (200-400 words)*

A critical ingredient in a reproducible workflow is keeping a detailed, up-to-date, and version-controlled lab notebook.
It is nearly unthinkable that a proper lab notebook for recording computational experiments could be kept without scripting all steps— pre-processing, running, post-processing—and automaticaly saving command-line inputs. 
In the project of this case study, we used four different CFD codes in batches of simulations spanning many parameter combinations, resulting in hundreds of runs.
The run times varied between 1 and 3 days and the numerical solutions each generated between 3.5 and 16 gigabytes of data.
Most of the simulations were run remotely on an HPC cluster at the George Washington University, and the solutions were then moved to several different local desktop machines for post-processing and storage.
The lab notebook proved to be vital for tracking all simulations and data.
Another aspect of this project that was very time consuming was becoming familiar with new software—it took even longer to familiarize ourselves with codes that offer poor users' documentation.
Finally, we also spent considerable time developing automated scripts for analyzing the numerical solutions resulting from different codes (producing different output formats).
These scripts, however, are essential to deliver reproducible computational experiments.


##### Key benefits
*Discuss one or several sections of your workflow that you feel makes your approach better than the "normal" non-reproducible workflow that others might use in your field. What does your workflow do better than the one used by your lesser-skilled colleagues and students, and why? What would you want them to learn from your example? (200-400 words)*

In the field of computational fluid dynamics, it can easily take six months or a year to develop software from scratch for solving a specific fluid-flow scenario.
On publishing the results, if the authors do not release the code and data used for the study, it leaves any reader hoping to reproduce the results facing a steep time investment.
Not surprisingly, studies attempting to reproduce previously published findings are rare.
As we have illustrated with our campaign to achieve full replication of our own previous study, there are severe pitfalls and challenges in fluid-flow simulations under unsteady, highly vortical regimes.
It is a distinct possibility that many published studies report wrong results.
As noted by Leek and Peng (2015), increasing the level of reproducibility of published studies will help uncover flawed research findings.
For this reason, the minimum level of reproducibility—making code and data available—is essential for increasing the confidence on any new scientific claims to knowledge generated computationally.
Going beyond sharing code and data, full automation and digital recording of experimental campaigns offer the best guarantee of being able to extract scientific value from computational experiments.

##### Key tools
*If applicable, provide a detailed description of a particular specialized tool that plays a key role in making your workflow reproducible, if you think that the tool might be of broader interest or relevance to a general audience. (200-400 words)*

We use the version-control hosting platform GitHub to support our reproducible workflow. 
GitHub greatly facilitates collaboration when developing numerical codes and documentation. 
The platform also allows creating wiki pages for users' documentation. 
We use GitHub to write manuscripts, to record our group-meetings, and to store teaching materials.
We also extensively use Python to automate analysis and post-processing.
Progress reports and summaries for discussion in group meetings are best presented using Jupyter notebooks, where textual media is combined with code and visualizations.
For a digital record of all steps taken in preparing a simulation and running it, bash scripting is essential.
We also use Travis CI for running automated testing of the codes whenever a change is to be merged into the main repository.

##### General questions about reproducibility

*Please provide short answers (a few sentences each) to these general questions about reproducibility and scientific research. Rough ideas are appropriate here, as these will not be published with the case study. Please feel free to answer all or only some of these questions.*

1) Why do you think that reproducibility in your domain is important?

In computational science, we use simulations and data analysis as tools for the creation and justification of scientific knowledge.
This process of knowledge creation, as in all science, must also produce evidence to justify itself.
Reproducibility is a way to provide grounds for trusting the scientific findings obtained computationally. 
Ensuring that a publication (along with the data used to generate the figures) is reproducible makes it easier for others to corroborate (or reject) a scientific hypothesis. 
Codes and data used to publish results should be version-controlled and open-source to facilitate reproducibility.
Donoho and co-authors (2009) mentioned that we develop codes so that they can be used again by strangers and defined strangers as "anyone who doesn't possess our current short-term memory" (including ourselves in some years).
We believe that reproducible research can also prevent scientists from "reinventing the wheel" by having to re-create complete software stacks to build from previously published work.

2) How or where did you learn the reproducible practices described in your case study? Mentors, classes, workshops, etc.

The group's PI, Prof. Lorena Barba, plays an active role in raising awareness about reproducible research. Incoming students joining our research lab must start by learning the different tools mentioned in the "Reproducibility PI Manifesto".
The [Software Carpentry Foundation](http://software-carpentry.org/) (through workshops and online ressources) also contributes to educate our group members and improve our workflow to achieve reproducible research.

3) What do you see as the major pitfalls to doing reproducible research in your domain, and do you have any suggestions for working around these? Examples could include legal, logistical, human, or technical challenges.

Reproducible research can be time-consuming, requiring rigorous methods and organization.
At various moments during the project, we had to pause and ask ourself if our research was currently reproducible.
Often, this was prompted by a conversation or questioning during group meetings.
In that sense, a strong collaborative culture in the research group, and beyond in the wider community of the discipline, are vital to instill reproducibility practices in computational researchers.
Lack of systematic and widespread educational programs that emphasize reproducible research is a serious obstacle.

4) What do you view as the major incentives for doing reproducible research?

Making your research more reproducible—e.g., providing reproducibility packages along with the manuscript—is a way of showcasing your skills, a medium for communicating research more transparently, and an invitation to give feedback on your work. 
If the research community is inclined to put more effort in doing reproducible research, it would prevent scientists from reinventing the wheel by rewriting software in order to build from your work.
In the long run, it saves resources to achive scientific knowledge growth, both at the level of a community and within a research group.

5) Are there any broad reproducibility best practices that you'd recommend for researchers in your field?

Again, we insist that automating all the computational workflow and diligently maintaining a lab notebook are fundamental to record your research.
We try to avoid GUIs as much as possible and prefer to script everything so that analysis can be automated, reproducible, and recorded.
This may be time-consuming but surely beneficial in the longer term of a research project.

6) Would you recommend any specific websites, training courses, or books for learning more about reproducibility?

* Barba, L. A. (13 December 2012). "Reproducibility PI Manifesto", 10.6084/m9.figshare.104539. Presentation for a talk given at the ICERM workshop "Reproducibility in Computational and Experimental Mathematics". Published on figshare under CC-BY.
* Donoho, D. L., Maleki, A., Rahman, I. U., Shahram, M., & Stodden, V. (2009). Reproducible research in computational harmonic analysis. Computing in Science & Engineering, 11(1), 8-18.
* Leek, J. T., & Peng, R. D. (2015). Opinion: Reproducible research can still be wrong: Adopting a prevention approach. Proceedings of the National Academy of Sciences, 112(6), 1645-1646.
* Madeyski, L., & Kitchenham, B. A. (2015). Reproducible Research–What, Why and How. Wroclaw University of Technology, PRE W, 8.
* Peng, R. D. (2011). Reproducible research in computational science. Science (New York, Ny), 334(6060), 1226.
* [Reproducible Research -- Coursera MOOC](https://www.coursera.org/learn/reproducible-research).
* Sandve, G. K., Nekrutenko, A., Taylor, J., & Hovig, E. (2013). Ten simple rules for reproducible computational research.
* [Software Carpentry](http://software-carpentry.org).
* Software Testing -- Udacity MOOC (https://www.udacity.com/).
* Stark, P. B. (2015). Science is "show me", not "trust me". [Blog post](http://www.bitss.org/2015/12/31/science-is-show-me-not-trust-me)
* Vitek, J., & Kalibera, T. (2011, October). Repeatability, reproducibility, and rigor in systems research. In Proceedings of the ninth ACM international conference on Embedded software (pp. 33-38). ACM.
