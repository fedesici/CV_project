# CV_project
## Repository Structure
```
/
├── Cv_project_neuronsel.ipynb #the main notebook with all the code
├── models/ #models pretrained, baseline and interpretable                        
│   ├── best_model__baseline.pth
│   └── best_model__interpretable.pth
├── nyu_data/ #folder which contain the dataset
├── imagenet/ #module to run fastdepth
├── fastdepth_model.py #model architecture
└── README.md
```

## Notebook overview
### Configuration  
This is the control panel for the project. Here is possible to modify the setup of the code and hyperparameters  
### Utility Functions  
Here the bins are discretized, and are selected the valid bins  
### Selectivity Loss  
Here is defined the class of the selectivity loss, which aims to maximize the Depth Selectivity score  
### Dataset  
In this section is handled the NYUDepth dataset, and created the dataloaders   
### Model Architecture & Training  
Where to load the model, MobileNetSkippAdd based on fastdepth, and to start the training loop  
### Evaluation  
This final section measures the model's performance both on depth and selectivity metrics, showing also the visual results of the depth predictions, activation maps and units response   

## Notebook use
### Dataset  
The notebook expects the following structure for the dataset  
```
nyu_data/
└── data/
    ├── nyu2_train/
    └── nyu2_test/
```  
In the CONFIG dictionary one can decide to enable or disable the selectivity in the following way:  
"ENABLE_SELECTIVITY": False: runs the standard baseline model, train the model only with L1Loss  
"ENABLE_SELECTIVITY": True: runs the interpretable model, adding the selectivity loss in the training  
SELECTIVITY_LAYER_NAME is a variable to select the layer where to apply selectivity  
### Training a Model  
Set your desired configuration in the CONFIG cell.  
If you want to start a new training run, make sure "RESUME_CHECKPOINT" is set to None, otherwise provide the checkpoint path  
Run all cells up to the training one  
The best model will be saved in the models/ directory, and the latest checkpoint will be saved in checkpoint/  
### Evaluating a Pre-trained Model  
If you just want to evaluate the pre-trained models provided in the models/ folder:  
Set the CONFIG to match the model you want to evaluate  
Run the cells until the network section, and skip the train sections  
Then run the evaluation section  
### Evaluation Part  
1. Depth Performance  
In the function visualize_predictions(), is it possible to show decide how many random samples to show by modifying num_samples   
2. Neuron Selectivity  
This part analyzes neurons in the target layer to see if they have learned to be selective for specific depth ranges. It generates bar plots where:  
Each plot represents one neuron.  
The x-axis represents different depth bins  
The y-axis shows the neuron's average activation response for that depth  
The orange bar highlights the depth bin that the neuron was assigned to target during training.  
Here is it possible to choose the number of units to show and also the number of batches where the average is calculated  
Activation Maps   
The function visualize_activation_maps, shows the activations map for a unit in a specific image  
To choose which neurons and images to inspect modify the following parameters:  
units_to_show: A list of neuron indices you want to see  
num_images: The number of random images to display from the test set.  
image_idx: To inspect one specific image, set its index and set num_images to 1.  
