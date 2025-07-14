# Super Mario Bros. Map Generation using GANs and Heuristic Selection

This project explores the generation of 2D maps for the classic game *Super Mario Bros.* using Generative Adversarial Networks (GANs). It combines a Wasserstein GAN (WGAN) with a heuristic-based selection algorithm to produce high-quality, playable levels that adhere to the original game's design principles. The project is the basis for the paper: "Geração de mapas para jogos em 2D utilizando Redes Generativas Adversariais."

-----

## Key Features

  * **GAN-Based Map Generation**: Utilizes a Wasserstein GAN (WGAN) to create novel map fragments.
  * **Heuristic Selection Algorithm**: Employs a sophisticated fitness function to filter and select the best-generated fragments based on structural coherence.
  * **A\* Playability Agent**: An A\* agent automatically evaluates the playability of each fragment and the final composed maps.
  * **Modular and Customizable**: The architecture allows for easy adjustments to the fitness function, GAN parameters, and evaluation metrics.
  * **Data-Driven**: Trained on the [Video Game Level Corpus (VGLC)](https://github.com/TheVGLC/TheVGLC), ensuring the generated content is grounded in the original game's design.

-----

## Methodology

The map generation process is a multi-stage pipeline designed to ensure both creativity and quality.

### 1\. Data Preprocessing

[cite\_start]The textual representations of *Super Mario Bros.* levels from the VGLC are used as the training data[cite: 116, 117]. These text files, where each character represents a game tile (e.g., 'X' for ground, 'E' for an enemy), are preprocessed by:

1.  [cite\_start]**Label Encoding**: Converting each tile character into a unique numerical value[cite: 125].
2.  [cite\_start]**Fragmentation**: The original, full-sized maps are divided into smaller `28x14` samples (fragments) to increase the volume of training data[cite: 129, 130]. [cite\_start]These are then padded to a `32x32` dimension to match the network's input requirements[cite: 131].

### 2\. WGAN for Fragment Generation

[cite\_start]A Wasserstein GAN (WGAN) was chosen for its training stability and ability to avoid common issues like mode collapse[cite: 160]. The architecture consists of:

  * [cite\_start]**Generator**: Uses ReLU and batch normalization to build map fragments from a latent space vector[cite: 161].
  * [cite\_start]**Critic (Discriminator)**: Employs Leaky ReLU and batch normalization to distinguish between real map fragments (from the dataset) and generated ones[cite: 139].

[cite\_start]The network was trained to generate **10,000 map fragments**[cite: 159].

### 3\. Heuristic Selection Algorithm

This is the core of the project's quality assurance. After the WGAN generates thousands of fragments, a selection algorithm filters them using a fitness function. [cite\_start]This function penalizes structural errors and rewards well-formed patterns[cite: 161, 187]. The fitness score is a sum of evaluations for:

  * [cite\_start]**Pipes (`C_fit`)**: Checks for the correct number and proper construction of pipes[cite: 189, 191].
  * [cite\_start]**Enemies (`I_fit`)**: Assesses the number and placement of enemies[cite: 199, 202].
  * [cite\_start]**Holes (`B_fit`)**: Evaluates the quantity and placement of gaps in the terrain[cite: 209].

[cite\_start]The algorithm selects the top **1,000 fragments** (10% of the total generated) for the next stage[cite: 186].

### 4\. A\* Agent for Playability Testing

[cite\_start]The final step uses an A\* agent to validate playability[cite: 223]. The process works as follows:

1.  **Fragment Validation**: The agent tests each of the 1,000 selected fragments individually. [cite\_start]Fragments that can be successfully traversed within a 60-second time limit are marked as "playable"[cite: 226, 227].
2.  **Map Composition**: The agent constructs full maps by sequentially concatenating 10 fragments. [cite\_start]After adding each new fragment, the agent re-evaluates the entire map to ensure it remains playable from start to finish[cite: 233, 234, 236].

-----

## Results

The combination of the WGAN with the heuristic selection algorithm yielded significant improvements over using the WGAN alone.

  * [cite\_start]**Enhanced Quality**: The selection algorithm effectively eliminated structural errors like malformed pipes and misplaced enemies, leading to maps with greater structural cohesion and adherence to the original game's patterns[cite: 291, 344].
  * [cite\_start]**Improved Playability**: The methodology produced a **25% increase** in the rate of usable, playable fragments[cite: 355].
  * [cite\_start]**Faster Evaluation**: The average time required for the A\* agent to evaluate the maps was **reduced by 15%**, indicating that the selected fragments were better structured and easier to navigate[cite: 355].
  * [cite\_start]**Balanced Difficulty**: While both methods tended to generate maps on the easier side, the selection algorithm produced a more balanced distribution of difficulties[cite: 345].

|  |  |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| [cite\_start]*Figure 1: Sample maps generated by the pure WGAN, showing structural inconsistencies. [cite: 302]* | [cite\_start]*Figure 2: Sample maps refined by the selection algorithm, demonstrating improved cohesion. [cite: 306]* |

-----

## How to Use

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/vmleroy/mario-maps-generation-gans-selection.git
    cd mario-maps-generation-gans-selection
    ```

2.  **Install dependencies:**
    It is recommended to use a virtual environment.

    ```bash
    pip install -r requirements.txt
    ```

3.  **Train the WGAN:**
    Run the training script to generate map fragments.

    ```bash
    python train_wgan.py
    ```

4.  **Run the Selection and Evaluation:**
    Execute the main script to apply the selection algorithm and have the A\* agent build and validate the final maps.

    ```bash
    python generate_maps.py
    ```

5.  **View the results:**
    The generated maps will be saved in the `output/maps` directory.

-----

## Future Work

Future research directions include:

  * [cite\_start]**Integrating Evolutionary Algorithms**: Using evolutionary methods to refine the GAN's training process for better pattern learning[cite: 365, 366].
  * [cite\_start]**Automating Weight Definition**: Employing heuristic methods like genetic algorithms or tools like Optuna to automatically optimize the fitness function's weights, which were set empirically in this study[cite: 368, 369].
  * [cite\_start]**Exploring Advanced Architectures**: Investigating more advanced GANs, such as StyleGAN, to capture more complex design patterns[cite: 370].

-----

## Citation

If you use this project for academic research, please cite the original paper:

Matos, V. L. R., Gomes, R. M., & Vila Nova, J. G. G. (2025). *Geração de mapas para jogos em 2D utilizando Redes Generativas Adversariais*. Trabalho de Conclusão de Curso, Centro Federal de Educação Tecnológica de Minas Gerais, Belo Horizonte, MG, Brasil.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

## Acknowledgments

  * [cite\_start]**Author**: Victor Le Roy Matos [cite: 2]
  * [cite\_start]**Advisor**: Prof. Rogério Martins Gomes [cite: 5, 421]
  * [cite\_start]**Co-advisor**: Prof. João Gabriel Gama Vila Nova [cite: 10, 424]
  * [cite\_start]This work was developed at the Departamento de Computação, CEFET-MG[cite: 3, 6].
