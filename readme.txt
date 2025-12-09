(GROUP 22)
Project overview: 
Reconstruction of 3D room using 2D sequence of images of a room. 

Data set:
Our data set comprises of 5 folders of images which were accessed through google drive in Colab notebook. 4 for walls and 1 for floor. Each has 7-15 overlapping images depending on the wall area.
- Wall1
- Wall2
- Wall3
- Wall4
- Floor

CODE STRUCTURE
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 

Pointcloud_cameraposes_generation.py
├── IMPORTS & SETUP
├── WEEK 1: Feature Matching & Analysis
│   ├── load_all_images_sequentially()
│   ├── display_original_image_grid()
│   ├── feature_mapping_between_pair()
│   └── create_feature_progression_analysis()
├── WEEK 2: Two-View Reconstruction
│   ├── two_view_reconstruction_stable()
│   ├── visualize_two_view_results()
│   └── run_week2_stable()
├── WEEK 3: Multi-View SfM with Bundle Adjustment
│   ├── StableMultiViewSfM class
│   │   ├── __init__()
│   │   ├── initialize_from_two_view()        
│   │   ├── extract_features()                
│   │   ├── find_2d_3d_correspondences()        
│   │   ├── estimate_camera_pose()            
│   │   ├── initialize_camera_from_previous() 
│   │   ├── triangulate_new_points_with_observations()  
│   │   ├── is_valid_point()                  
│   │   ├── run_bundle_adjustment_proper()    
│   │   ├── clean_points_after_ba()           
│   │   ├── visualize_reconstruction()        
│   │   ├── run_pipeline_with_ba()            
│   │   └── save_results() 
│   └── run_week3_with_proper_ba()
├── MAIN PIPELINE FUNCTION
    └── run_complete_pipeline_with_ba()
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
merge_auto_all.py
├── IMPORTS & SETUP
│ ├── import os, json, numpy as np
│ └── import open3d as o3d
│ ├── DATA_DIR, OUTPUT_DIR, TARGET_POINTS, VOXEL_SIZE
│ └── WALL_COLORS, ICP_THRESHOLDS

├── CAMERA HANDLING
│ ├── load_camera_poses(path)
│ └── transform_cameras(cam_list, T)

├── POINT CLOUD HANDLING
│ ├── load_and_densify(path, target_points=TARGET_POINTS)
│ ├── compute_fpfh(pcd)
│ ├── run_ransac(source, target)
│ └── refine_icp(source, target, init_trans=np.identity(4), threshold=0.1, use_point_to_plane=False)

├── CAMERA CONNECTIONS
│ └── compute_camera_connections(cameras, k=3)

├── WALL MERGING
│ └── merge_walls(wall_files, camera_files)
│ ├── load/densify each wall
│ ├── assign wall colors
│ ├── predefined transformations for Wall2
│ ├── RANSAC alignment with merged cloud
│ ├── ICP refinement
│ ├── transform cameras & assign unique indices
│ └── remove outliers from merged cloud

├── VISUALIZATION
│ └── visualize(merged_pcd, cameras)
│ ├── create window & add merged point cloud
│ ├── add camera spheres
│ └── set view control parameters

├── MAIN EXECUTION
│ ├── define wall_files & camera_files
│ ├── merged_walls, merged_cams = merge_walls(...)
│ ├── save merged point cloud to OUTPUT_DIR
│ ├── compute connected cameras & save JSON/npy files
│ └── visualize(merged_walls, merged_cams)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
3D_Roomtour.html 
├── HTML STRUCTURE
│ ├── <head>
│ │ ├── meta tags (charset, viewport)
│ │ └── title & <style> for body, container, canvas, overlay, cursor, caption, toggle button
│ └── <body>
│ └── #tourContainer
│ ├── #tourCaption
│ ├── #imageOverlay
│ ├── #cursor
│ └── #toggleBtn

├── IMPORTS & SETUP (JavaScript Module)
│ ├── import * as THREE, OrbitControls, PLYLoader
│ ├── scene, camera, renderer, controls
│ ├── cameraNodes, currentNodeIndex, preloadedImages
│ ├── raycaster, mouse, cameraNodeGroup, pointCloud
│ ├── panoramaCylinder, cameraSphereRadius, is360Mode, isTransitioning
│ └── DOM references: imageOverlay, cursorEl, captionEl, toggleBtn

├── UTILITY FUNCTIONS
│ ├── smoothstep(t)
│ ├── showImage(path)
│ ├── crossfadeImages(newImagePath, opacityDurationInSeconds)
│ └── onWindowResize()

├── INITIALIZATION & LOADING (init())
│ ├── Scene & Camera Setup
│ ├── Renderer & DOM attachment
│ ├── OrbitControls (for 360° mode)
│ ├── Raycaster & Camera Node Group
│ ├── Load Merged Point Cloud (merged_walls.ply)
│ ├── Load Merged Camera Poses (merged_cameras.json)
│ ├── Preload Images for Cameras
│ ├── Create camera spheres in scene
│ ├── Set initial camera node (Photosynth style)
│ └── Create 360° panoramic cylinder with texture

├── TOGGLE 360° / PHOTOSYNTH MODE (toggleBtn click)
│ ├── Switch visibility between panoramaCylinder and camera nodes/point cloud
│ ├── Enable/disable OrbitControls
│ └── Update container height and caption text

├── CAMERA NAVIGATION & INTERACTIONS
│ ├── Keyboard arrows → startTransition()
│ ├── Mouse dragging → left/right transitions
│ ├── Mouse click → onCanvasClick() to follow connected camera nodes
│ └── Mouse move → update cursor position and raycaster hover effect

├── PHOTOSYNTH FUNCTIONS
│ ├── setCameraToNode(index) → move camera to node, set quaternion, show image
│ └── startTransition(targetIndex) → smooth LERP/SLERP position & rotation with crossfade

├── ANIMATION LOOP
│ └── animate() → requestAnimationFrame, controls.update (if 360 mode), renderer.render
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOLDER STRUCTURE

phase3_project/
├── data/
│ ├── Wall1_cameraposes.json
│ ├── Wall1_cloudpoint.ply
│ ├── Wall2_cameraposes.json
│ ├── Wall2_cloudpoint.ply
│ ├── Wall3_cameraposes.json
│ ├── Wall3_cloudpoint.ply
│ ├── Wall4_cameraposes.json
│ └── Wall4_cloudpoint.ply

├── scripts/
│ ├── merge_auto_all.py
│ ├── ScreenCamera_2025-12-08-19-33-48.json
│ ├── ScreenCapture_2025-12-08-19-33-48.png
│ └── Stitching_images.ipynb

├── web/
│ ├── images/
│ │ └── … (all image files used in 3D Room Tour)
│ ├── lib/
│ │ ├── three.module.js
│ │ ├── OrbitControls.js
│ │ └── PLYLoader.js
│ ├── 3D_Roomtour.html
│ ├── full_room_panorama_new.jpg
│ ├── merged_cameras.json
│ ├── merged_cameras.npy
│ ├── merged_walls.ply
│ ├── package-lock.json
│ └── package.json