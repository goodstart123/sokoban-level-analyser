# Sokoban Level Analysis Project

## Overview

This project analyzes Sokoban levels using computer vision techniques to evaluate level design, complexity, and player experience. The system processes an image of a Sokoban level, segments it into tiles, identifies different tile types, and then analyzes the level's structure based on object density, pathway analysis, symmetry, and player-to-box distances. A comprehensive visual report is generated to summarize the findings.

## Methods and Libraries

The project uses the following methods and libraries:

### Computer Vision:

*   **Image Processing:**
    *   **OpenCV (cv2):** Used for image loading, color space conversion, connected components analysis, and color map application.
    *   **NumPy:** For numerical operations on images and arrays, image manipulation, distance computations.
    *   **Matplotlib:** Used for visualizing the original and segmented images and generated the visual report.
*   **Tile Segmentation:** The image is divided into a grid of tiles based on predefined `block_w` (width) and `block_h` (height) parameters.
*   **Feature Detection:** For each tile, the standard deviation of pixel values (std_dev) and the average color (avg_color) are calculated.
*   **Classification Heuristic:** `classify_tile` function uses these features to assign a tile type (e.g., Floor, Wall, Storage, Box, Player, Unknown) based on a set of thresholds and color matching.
*   **Sokoban Tile Types:** The `COLOR_MAP` dictionary defines the color representation for each Sokoban tile type.
*   **Level Design Evaluation:**
    *   **Object Counting:** The `evaluate_level_design` function counts the number of each tile type.
    *   **Object Density:** The density of each object type relative to the total number of tiles is calculated.
    *   **Pathway Analysis:** The `visualize_pathways` function utilizes connected component analysis to identify and visualize connected open spaces.
    *   **Symmetry Analysis:** `evaluate_level_design` checks for horizontal and vertical symmetry by comparing the grid with its flipped versions.
    *   **Distance Analysis:** The `visualize_distances` function visualizes distances between the player and each box.
    *   **Difficulty and Variety Scores:** `evaluate_level_design` calculates a difficulty score based on wall density, open spaces, box distances, and symmetry, and a variety score based on the number of unique object types.

### Python Libraries:

*   **IPython:** For interactive features and getting the current ipython session.
* **google.colab:** for uploading files from user's system to the colab environment.
*   **Matplotlib (pyplot):** For creating plots and charts in the visual report.
*   **NumPy:** For efficient array operations and math calculations.
*   **SciPy (spatial.distance):** For calculating the Euclidean distance between player and box positions.
* **cv2:** for computer vision algorithms like image processing, connected components analysis, and color map application.

## System Description

The system workflow is as follows:

1.  **Image Upload:** The user uploads an image of a Sokoban level.
2.  **Image Segmentation:** The `process_sokoban_level` function divides the image into a grid of tiles.
3.  **Tile Classification:** Each tile is analyzed, and a type is assigned based on the `classify_tile` function's heuristic.
4.  **Grid Analysis:**
    *   The `evaluate_level_design` function calculates object counts, densities, pathway open spaces, symmetry, and the average box distance to the player.
    *   It also provides a difficulty and a variety score.
5.  **Visual Report Generation:** The `generate_visual_report` function creates a report with:
    *   Bar chart of object counts.
    *   Pie chart of object densities.
    *   Visualization of connected open spaces (pathways).
    *   Visualization of distances from the player to the boxes.
6. **Report output:** Report is shown to the user.

## Challenges and Solutions

*   **Accurate Tile Classification:**
    *   **Challenge:** Initial classification heuristics were not robust enough to handle variations in lighting, image quality, and minor color differences.
    *   **Solution:** The `classify_tile` function was improved by:
        *   Relaxing the `std_dev` and `avg_color` thresholds to allow for more tolerance.
        *   Grouping similar tiles (e.g., Outer Floor and Floor) into the same category.
        *   Adding `np.all(np.isclose(...))` to compare RGB values with a tolerance (`atol`) for more flexible matching.
*   **Handling Variations in Input Images:**
    * **Challenge**: Input images could have varying image qualities, lighting conditions, and slight color differences. This could result in poor segmentation and misclassification of tiles.
    * **Solution**: By using thresholds instead of direct comparisons when classifying, the model can handle varying images better.
*   **Pathway and Distance Analysis:**
    *   **Challenge:** Accurately representing and visualizing pathways and distances.
    *   **Solution:** Used the `cv2.connectedComponentsWithStats` function to identify connected regions and the `cv2.applyColorMap` function to visualize distances.
*   **Visual Report Customization:**
    *   **Challenge:** Creating a report that is both informative and visually appealing.
    *   **Solution:** Used `matplotlib` to create a grid of four subplots, each presenting a different aspect of the level analysis.

## System Setup and Instructions

1.  **Google Colab Environment:** This project is designed to run in a Google Colab environment.
2.  **Library Installation:** All libraries used are standard Colab libraries, no special installation is required.
3.  **Image Upload:**
    *   Run the code cell with the `files.upload()` command.
    *   Click the "Choose Files" button and select the Sokoban level image from your local machine.
4.  **Run the code cells:**
    *   After image is uploaded, run all cells in order.
5.  **View Results:** After running all the cells, you will be able to see:
    *   Original Level: The original uploaded image.
    *   Segmented Level: A color-coded representation of the classified tiles.
    *   Level Design Evaluation: Text output with object counts, densities, symmetry, distances, and scores.
    *   Visual Report: A figure with four subplots showing object counts, densities, pathway analysis, and distance analysis.
