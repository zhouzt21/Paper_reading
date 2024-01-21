## Cgi scene

#### CGI-Scene: Controllable Generation of Open-Vocabulary Interactive Scenes

Generating interactive 3D scenes:

* other: depend on **specialized training datasets or manually defined rules**
  * detailed object placement rules (visual balance and clearance as cost functions for optimization) 
  * large-scale scene layout datasets with annotated object categories and bounding boxes

* cgi: pipeline -- leverages the power of generative models to enable Controllable Generation of Interactive 3D Scenes
  * producing diverse scenes with varied content in a controllable manner

* create 3D scenes from 2D images
  * other: resulting scenes, composed of volumetric data or single meshes, lack interactivity(most layout synthesis methods sample objects after layout generation, cannot control the visual style of selected objects)
  * ours: facilitates the creation of open-vocabulary 3D scenes from a single reference image

### Pipeline

* use a segmentation model to identify each object in the scene.
* each object identified:  vision-language model -- detailed descriptions (object’s color, shape,  texture)
* retrieve the most similar models from a 3D asset database using multi-modal features
* perform object pose estimation and optimization to position candidate object
* select the most suitable candidate for each identified object
* apply physical constraints to the entire scene :  ensure a physically plausible layout

具体细节：

* reference image analysis（camera parameters, segmentation results, and an estimated depth map,）
  * **Oneformer & Zoedepth**: derive panoptic segmentation and depth maps.
  * leverage GPT-4 to group all detected objects into three  classes
    * Ground Objects（substantial objects positioned on the floor），Surface Objects（smaller objects situated on top of the ground objects），Environment Objects
  * **Perspective Fields** for single-image camera calibration (intrinsic vertical 213 field of view  & extrinsic camera rotation)

* 3D shape retrieval
  * which learns 3D shape representations aligned with CLIP text and image features, enabling cross-modal 3D shape retrieval
  * **OpenShape**: learns 3D shape representations aligned with CLIP
  * **LLaVA**（vision-language model）： generate a textual description, extract CLIP features
    * ![image-20240103103128964](.\asset\3d_scene_cgi_clip.png)

* object pose estimation

  * Object Orientation Prediction
    * **PoseContrast**: class-agnostic method for viewpoint estimation of objects.
      * predefine angles -- PoseContrast estimate the confidence

  * Object Translation and Scale Optimization
    * simulated annealing algorithm
      * ![image-20240103104120137](.\asset\3d_scence_cgi_opt.png)

* Physics-Constrained Scene Optimization
  * Convex  decomposition: determine the collision shape, mass and inertia

![image-20240103102252229](.\asset\3d_scene_cgi)

* 

### 标记

* S = o1, o2, . . . , on；  
* (UIDi, ci, ti, ri, si)，UIDi denotes the identifier in the source database





entail 蕴含



## Part-level Scene Reconstruction Affords Robot Interaction 2023

* traditional scene reconstruction methods primarily focus on generating static scenes, represented by sparse landmarks [1, 2], occupancy grids [3], surfels [4, 5], volumetric voxels [6, 7], or semantic objects
  * lack the ability to capture the dynamic nature of robot operations and limit the complexity of tasks that can be performed

* unsatisfactory CAD replacements

* limited number of CAD models

We propose a part-level reconstruction strategy that focuses on reconstructing scenes by replacing object CAD models at the part level instead of the object level.

* a semantic point cloud completion network to decompose and complete each noisily segmented object into parts.
* we perform part-level CAD replacement,
  * aligning primitive shapes to individual parts 
  * estimating their kinematic relations.
    * kinematics-based scene graph

#### KINEMATICS-BASED SCENE REPRESENTATION

* extend the contact graph (cg)   by incorporating scene entity parts and their kinematic information.

  * parse tree organizes scene entity nodes (V ) hierarchically based on supporting relations (S)
    * object node in V includes attributes describing its semantics and geometry
  * the proximal relations capture the relationships between objects
    * supporting and proximal relations impose constraints to ensure physically plausible object placements.

  * an additional attribute denoted as pt^p
    * represents a per-object part-level parse tree
    * organizes part entities (V^p) along with their kinematic relations (J ).

* part entity nodes  V^p , represents all part entities within an object

  * $$
    v_p=<l,c,M,\Pi>\\
    \Pi= (\pi^k, U^k)
    $$

  * unique part instance label (l),    part semantic label (c),       a geometry model (M ) in the form of a triangular mesh or point cloud,       a set of surface planes (Π)

  * U^k 3D vertices defining a polygon that outlines the plane  π^k, π^k is represented by a homogeneous vector [n^k^T , d^k]^T     R4 in projective space

* kinematic relations  J = Jp,c   , represents the parametric joints between part entities within an object

  * $$
    J_{p,c}=<t_{p,c},T_{p,c},F_{p,c}>
    $$

  * between a parent part (vp) and a child part (vc).

  * the parent-to-child transformationT,    joint axis (Fp,c,  joint encodes the joint type

    * fixed joint: Represents a rigid connection between two parts, such as a table top and a table leg. 
    *  prismatic joint: Indicates that one part can slide along a single axis with respect to the other, as seen in an openable drawer and a cabinet base.
    * revolute joint: Represents a joint where one part can rotate around a single axis in relation to another part, like the door and base of a microwave.

  * Establishing a kinematic relation:  requires   them  in contact with satisfying the following constraints

    * $$
      D pπi p, Ui pq P Πp, pπj c, Uj c q P Πc,
      $$

    * 



#### PART-LEVEL CAD REPLACEMENT

##### A. Individual part alignment

* 

* where hpMiq is a set of evenly sampled points on the surface of the CAD model Mi, dP puq is the distance from a sampled point u to the closest point in P , and Ti  ̋ u is the position of point u after applying transformation Ti.
* iterative closest point method
* M  ̊ is the primitive candidate with the smallest minimum total distance among all candidates, and T  ̊ ind is the corresponding optimal transformation   



##### B. Kinematic relation estimation

estimate the parentchild contact relations and kinematics between parts to obtain a per-object part-level parse tree pt^p

1) We acquire its part-level semantic label c, instance label l, and point cloud P . 
2) We replace its point cloud P with a primitive shape M , as described in Sec. III-A. 
3) We extract surface planes Π from M by iteratively applying RANSAC





We solve this optimization problem i

* construct a directed graph with nodes in V p
* optimal parent-child relations





##### C. Spatial refinement among parts

* perform a refinement process to adjust transformations in J so that parts forming parent-child pairs are better aligned.

* spatial refinement algorithm



