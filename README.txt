=======================================================

  COMPILES: COMPutIng anaLytic positivE Steady states

=======================================================

Matlab was used to develop the functions used here.



=========
Functions
=========The function steadyState returns the steady state solution of a chemical reaction network (CRN) parametrized by rate constants (and/or sigma's). The free parameters and conservation laws are also listed after the solution. If there are subnetworks that could not be solved because translation is taking too long, solving the subnetwork is skipped and a message saying it could not be solved is displayed. In this case, the parametrization of the steady state of the entire network is not completed but the lists of solved and unsolved subnetworks are displayed. In the case where all subnetworks are solved but the solution of the entire network cannot be parametrized in terms of the free parameters (due to two or more species dependent on each other), the solution returned is in terms of both free parameters and other "nonfree" species. If there are subnetworks containing only 1 reaction, it means that there are species with 0 steady steady; hence, the network has not positive steady state and a message appears saying so. The output variables 'equation', 'species', 'free_parameter', 'conservation_law', and 'model' allow the user to view the following, respectively:

   - List of parametrization of the steady state of the system
   - List of steady state species of the network
   - List of free parameters of the steady state
   - List of conservation laws of the system
   - Complete network with all the species listed in the 'species' field of the structure 'model'

steadyState uses the following functions:     1. indepDecomp
           - OUTPUT: Decomposes a network into its finest independent decomposition. The output variables 'model', 'R', 'G', and 'P' allow the user to view the completed structure, matrix of reaction vectors of the network, undirected graph of R, and partitions representing the decomposition of the reactions, respectively.
           - INPUT: model: a structure representing the CRN (see details below)

     2. analyticSolution
           - OUTPUT: Solves for the parametrized steady state solution of a network. The output variables 'param', 'B_', 'sigma_', 'k', 'sigma', 'tau', 'model', and 'skip' allow the user to view the list of parametrization of the species of the system, updated list of tau's already used in parametrization, updated list of sigma's already used in parametrization, list of rate constants of the system, list of sigma's of the system, list of tau's of the system, completed structure, and the indicator for steadyState to skip solving the subnetwork, respectively.
           - INPUTS
                - model: a structure representing the CRN (see details below)
                - P_: list of reaction numbers from the independent decomposition
                - B_: list of tau's already used in parametrization
                - sigma_: list of sigma's already used in parametrizationThe following are functions used in analyticSolution and the functions inside it:     1. modelSpecies
           - OUTPUT: Creates a complete model structure where the species list is filled out. The output variables 'model' and 'm' allow the user to view the complete model and the number of species, respectively.
           - INPUT: model: a structure representing the CRN (see details below)

     2. stoichMatrix           - OUTPUT: Creates the stoichiometric matrix of the system. The output variables 'N', 'reactant_complex', 'product_complex', and 'r' allow the user to view the stoichiometric matrix, matrix of reactant complexes, matrix of product complexes, and number of reactions, respectively.
           - INPUTS
                - model: a structure representing the CRN (see details below)
                - m: number of species

     3. GCRN
           - OUTPUT: Creates the generalized chemical reaction network (GCRN) of a translated network. The output variables 'stoichiometric_complex_reactant', 'stoichiometric_complex_product', 'kinetic_complex_reactant', 'kinetic_complex_product', 'model', and 'skip' allow the user to view the reactant stoichiometric complex of the GCRN, product stoichiometric complex of the GCRN, kinetic complex associated with the reactant stoichiometric complex, kinetic complex associated with the product stoichiometric complex, completed structure, and the indicator for steadyState to skip solving the subnetwork, respectively.
           - INPUT: model: a structure representing the CRN (see details below)

     4. findTranslations
           - OUTPUT: Determines the different weakly reversible and deficiency zero translations of a CRN. The output variables 'Solution', 'Index', and 'skip' allow the user to view the reactant complexes and product complexes of each translation, the permutation of the reaction numbers between the original CRN and the translation, and the indicator for steadyState to skip solving the subnetwork, respectively.
           - INPUTS
                - reactant_complex: matrix of reactant complexes
                - product_complex: matrix of product complexes

     5. kineticDeficiency
           - OUTPUT: Determines the kinetic deficiency of a GCRN. The output variable 'kinetic_deficiency' allows the user to view the kinetic deficiency.
           - INPUTS
                - kinetic_complex_reactant: matrix of kinetic reactant complexes
                - kinetic_complex_product: matrix of kinetic product complexes

     6. linkageClass
           - OUTPUT: Determines the number of linkage and strong linkage classes. The output variables 'l' and 'sl' allow the user to view the number of linkage classes and strong linkage classes, respectively.
           - INPUTS
                - reactant_complex: matrix of reactant complexes
                - product_complex: matrix of product complexes

     7. decimalToBinary
           - OUTPUT: Converts a number in the decimal system (i.e., base 10) to the binary number (i.e., base 2). The output variable 'binary_num' allows the user to view the number in binary system.
           - INPUTS
                - decimal_num: a number in decimal system
                - bin_len: desired length of binary number

     8. positiveSolution
           - OUTPUT: Checks the existence of the vector with positive entries in the kernel of a given matrix. The output variables 'pos_sol' and 'pos_sol_TF' allow the user to view the positive solution to Aeq * x = 0 and whether or not Aeq * x = 0 has a positive solution, respectively.
           - INPUT: Aeq: a matrix

     9. translation
           - OUTPUT: Generates all possible translated networks and check weak reversibility and zero deficiency. The output variables 'Solution, 'Index', and 'skip' allow the user to view the list of reactant complexes and product complexes of each translation (represented by each row), the list of permutations of the reaction numbers between the original CRN and the translation, and the indicator for steadyState to skip solving the subnetwork, respectively.
           - INPUTS
                - reactant_complex: matrix of reactant complexes
                - product_complex: matrix of product complexes
                - max_order: maximum number of species in a complex

     10. directedSpanTreeTowards
           - OUTPUT: Creates a list of directed spanning trees towards the indicated root vertex of a directed graph. The output variable 'spanTree' allows the user to view the edges of the directed spanning tree towards the root vertex.
           - INPUTS
                - G: directed graph with vertices and edges
                - r: root vertex

analyticSolution and directedSpanTreeTowards use the following classes and function:     1. graph_ (class)
           - OUTPUT: Returns a digraph with 'V' vertices and corresponding default properties. The edges of the digraph are equally weighted.
           - INPUT: V: number of vertices in the digraph
           - This class has 8 functions inside it:
                a. noKnownEdge: To check if the edge we are trying to add is NOT YET recorded
                b. addEdge: To add an edge to the digraph
                c. removeEdge: To remove an edge from the digraph
                d. connectedVertex: To check if a vertex is a node of any edge
                e. connectedVertices: To check how many vertices of the graph are connected (but does not indicated number of components)
                f. depthFirstSearch: To try to go through each vertex starting with vertex v, then determine its parent vertex
                g. displayGraph: To display the edges of the graph
                h. plotGraph: To plot the graph

     2. grow (function; only in directedSpanTreeTowards)           - OUTPUT: Returns the spanning trees away from the root vertex. The output variables 'spanTree' and 'spanTreeCount' allow the user to view the list of edges of the directed spanning tree towards the root vertex and the count of spanning trees rooted at G.root_vertex, respectively
           - INPUTS
                - V: number of vertices in the graph
                - G: given digraph
                - T: subtree for spanning tree generation
                - L: latest spanning tree found
                - F: list of all edges directed from vertices in T to vertices not in T
                - spanTreeCount: number of spanning trees
                - spanTree: list of edges of the directed spanning tree towards the root vertex
     3. edge (class; used in graph_ and grow)           - OUTPUT: Returns a directed edge with indicated starting and ending vertex numbers. It adds a directed edge from 'from_node' to 'to_node' to a directed graph created using the class graph_.
           - INPUTS
                - from_node: starting vertex number (natural number)
                - to_node: ending vertex number (natural number)
     4. vertex (class; used in graph_)
           - OUTPUT: Returns a digraph with 'V' vertices and corresponding default properties.
           - INPUT: V: number of vertices in the digraph


=====
Notes
=====

For the function steadyState:

     1. It is assumed that the CRN has mass action kinetics.

     2. The algorithm first decomposes the network intro its finest independent decomposition. Then the steady state of the species in each subnetwork is solved. Finally, these solutions are combined to get the solution to the entire network.

     3. Decomposition into the finest independent decomposition comes from [2].

     4. Translation of network comes from [4].

     5. Parametrization of steady state solution comes from [5].

     6. Computation of directed spanning trees toward vertices come from [1].

     7. Sometimes, when the solution could not be expressed in terms of free parameters only, renaming the variables can solve the problem: upon doing this, some subnetworks may be solved for different species depending on the variable assigned to them since the selection of species to solve is based on alphabetical order (see Examples 10 and 10b, and Examples 11 and 11b).

     8. Ideas for some parts of the code was motivated by [10].

     9. Make sure all 4 classes/function are in the same folder/path being used as the current working directory.

     10. The classes and functions for directedSpanTreeTowards are converted from Python to Matlab. The Python code can be found in https://github.com/ricelink/finding-all-spanning-trees-in-directed-graph.



=================================
How to fill out 'model' structure
=================================

'model' is the input for the function steadyState. It is a structure, representing the CRN, with the following fields:

   - id: name of the model
   - species: a list of all species in the network; this is left blank since incorporated into the function is a step which compiles all species used in the model
   - reaction: a list of all reactions in the network, each with the following subfields:
        - id: a string representing the reaction
        - reactant: has the following further subfields:
             - species: a list of strings representing the species in the reactant complex
             - stoichiometry: a list of numbers representing the stoichiometric coefficient of each species in the reactant complex (listed in the same order of the species)
        - product: has the following further subfields:
             - species: a list of strings representing the species in the product complex
             - stoichiometry: a list of numbers representing the stoichiometric coefficient of each species in the product complex (listed in the same order of the species)
        - reversible: has the value true or false indicating if the reaction is reversible or not, respectively
        - kinetic: has the following further subfields:
             - reactant1: a list of numbers representing the kinetic order of each species in the reactant complex in the left to right direction (listed in the same order of the species)
             - reactant2: a list of numbers representing the kinetic order of each species in the reactant complex in the right to left direction (listed in the same order of the species) (empty if the reaction is not reversible)

To fill out the 'model' structure, write a string for 'model.id': this is just to put a name to the network. To add the reactions to the network, use the function addReaction where the output is 'model'. addReaction is developed to make the input of reactions of the CRN easier than the input in [10]:

   addReaction
      - OUTPUT: Returns a structure called 'model' with added field 'reaction' with subfields 'id', 'reactant', 'product', 'reversible', and 'kinetic'. The output variable 'model' allows the user to view the network with the added reaction.
      - INPUTS
           - model: a structure, representing the CRN
           - id: visual representation of the reaction, e.g., reactant -> product (string)
           - reactant_species: species of the reactant complex (cell)
           - reactant_stoichiometry: stoichiometry of the species of the reactant complex (cell)
           - reactant_kinetic: kinetic orders of the species of the reactant complex (array)
           - product_species: species of the product complex (cell)
           - product_stoichiometry: stoichiometry of the species of the product complex (cell)
           - product_kinetic: "kinetic orders" of the species of the product complex, if the reaction is reversible (array); if the reaction in NOT reversible, leave blank
           - reversible: logical; whether the reaction is reversible or not (true or false)
      * Make sure the function addReaction is in the same folder/path being used as the current working directory.



========
Examples
========

15 examples are included in this folder:

   - Example 0: A reaction network with mass action kinetics [3]

   - Example 1: A network that does not need a phantom edge

   - Example 2: Histidine Kinase Network [5]

   - Example 3: A reaction network with mass action kinetics [3]

   - Example 4: Subnetworks 1 and 2 of the zigzag model of plant-pathogen interactions [3]

   - Example 5: Zigzag Model [6]

   - Example 6: Insulin metabolic signaling network [7]

   - Example 7: EnvZ-OmpR Signaling Pathway [5]

   - Example 8: MAPK Model [6]

   - Example 9: Shuttled WNT Signaling [5]

   - Example 10: Simplest model for the CRISPRi toggle switch [9]

   - Example 10b: Example 10 with change in variables

   - Example 11: Extended model for the CRISPRi toggle switch [9]

   - Example 11b: Example 11 with change in variables

   - Example 12: Calvin cycle [8]

Examples 0 to 6, 10b, and 11b include checker files to show that the solutions returned by the function are indeed steady states of their respective ordinary differential equations.



===================
Contact Information
===================

For questions, comments, and suggestions, feel free to contact me at pvnlubenia@yahoo.co.uk.


- Patrick Lubenia (28 September 2022)



==========
References
==========

   [1] Gabow H and Meyers E (1978) Finding all spanning trees of directed and undirected graphs. SIAM J Comput 7(3):280-287. https://doi.org/10.1137/0207024

   [2] Hernandez B, De la Cruz R (2021) Independent decompositions of chemical reaction networks. Bull Math Biol 83(76):1–23. https://doi.org/10.1007/s11538-021-00906-3

   [3] Hernandez B, Lubenia P, Johnston M, Kim J (2022) Deriving analytic positive steady states of chemical reaction systems via network decomposition and network translation (in preparation)

   [4] Hong H, Hernandez B, Kim J, Kim JK (2022) Computational translation framework identifies biochemical reaction networks with special topologies and their long-term dynamics (submitted)

   [5] Johnston M, Mueller S, Pantea C (2019) A deficiency-based approach to parametrizing positive equilibria of biochemical reaction systems. Bull Math Biol 81:1143–1172. https://doi.org/10.1007/s11538-018-00562-0

   [6] Johnston M, Burton E (2010) Computing weakly reversible deficiency zero network translations using elementary flux modes. Bull Math Biol 81:1613–1644. https://doi.org/10.1007/s11538-019-00579-z

   [7] Lubenia P, Mendoza E, Lao A (2022) ﻿Reaction network analysis of metabolic insulin signaling. Bull Math Biol 84:129. https://doi.org/10.1007/s11538-022-01087-3

   [8] Poolman M, Fell D, Thomas S (2000) Modelling photosynthesis and its control. J Exp Bot 51:319-328. https://doi.org/10.1093/jexbot/51.suppl_1.319

   [9] Santos-Moreno J, Tasiudi E, Stelling J, Schaerli Y (2020) Multistable and dynamic CRISPRi-based synthetic circuits. Nat Commun 11(2746):1-8. https://doi.org/10.1038/s41467-020-16574-1

   [10] Soranzo N, Altafini C (2009) ERNEST: a toolbox for chemical reaction network theory. Bioinform 25(21):2853–2854. https://doi.org/10.1093/bioinformatics/btp513