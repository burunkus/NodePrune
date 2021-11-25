# Instructions
Name: Ebuka Johnbosco Okpala

Paper title: NodePrune: Sparsifying Neural Networks via Iterative Pruning

Implementation chioce: A - implemented using Object Oriented Programming

Note: This project is inspired by [DropNet](https://github.com/tanchongmin/DropNet) and improves on its MLP task. 

#### Required Installations
- tensorflow >= v2 or tensorflow-gpu (Used this but either will work)
- keras (now integrated in tensorflow as tensorflow.keras)
- numpy
- copy
- sklearn
- matplotlib

#### Setup requirements
- At the root folder of this project create a folder named 'Caches' to store the model caches. If you clone this repo you don't have to create the folder as I have already created it. 

#### Files in this project
There are two files contained in this project, a Jupyter Notebook named `dropnet_oop.ipynb` and a python file named `dropnet_oop.py`. Both contain the same code though the Jupyter Notebook is more pleasing to the eye. 

#### Design 
In each file, there is a super class named `NodePrune` from which other base classes inherit from. `NodePrune` contains methods that are required for all experiments, each of the base classes that inherits from `NodePrune` creates a neural network model as discussed in the paper. There were three base models, model a, b and c and bigger models - VGG19 and ResNet18. In code, a Multi-layer perceptron (Model A) is represented as `class MLPModelA`, a CNN model (Model B) as `class CnnModelB`, a CNN model, Model C as `class CnnModelC`, ResNet18 as `class ResNet18` and VGG19 as `class Vgg19`. Subsequently, each of the experiment in the paper is a class with a `run` method. For example, a CNN experiment on the MNIST dataset performed on Model B having two convolution layers each with 64 filters is represented as `class CnnMnistExperimentModelB_64_64()`. 

#### Running the python file
* Start by creating a virtual environment
* Open terminal
* shh into your palmetto account 
* Load the anaconda module: `module add anaconda3/2019.10-gcc/8.3.1`
* Create a virtual environment: `conda create --name put_your_env_name_here python=3.8`
* Type yes to accept the installation
* Activate the environment: `source activate put_your_env_name_here`
* Install the packages above:
  * Install tensorflow-gpu: `conda install -c anaconda tensorflow-gpu`
  * The above should have installed numpy
  * There is no need of installing `copy` it comes with Python
  * Install sklearn: `conda install -c anaconda scikit-learn`
  * Install matplotlib: `conda install -c conda-forge matplotlib`
  * For each of the installs, always accept the installations by typing `y` so it does not get killed on palmetto
* Navigate to the directory where you have my code - if you cloned this repo the files should all be in a folder named `Dropnet`. So cd into it: `cd Dropnet`
* Create a batch job (a .pbs file) named for example `test_Ebuka_Okpala.pbs`
  * The file should contain the following
  ```
  #PBS -N test_Ebuka_code_job
  #PBS -o /home/put_your_username_here/Dropnet/Ebuka_output_file.txt
  #PBS -e /home/put_your_username_here/Dropnet/Ebuka_error_file.txt
  #PBS -l select=1:ncpus=16:ngpus=1:gpu_model=k40:mem=125gb:interconnect=fdr,walltime=72:00:00
  #PBS -m abe
  #PBS -M put_your_email_here@clemson.edu

  module add anaconda3/2021.05-gcc/8.3.1

  source activate put_your_env_name_here

  cd /home/put_your_username_here/Dropnet/
  python dropnet_oop.py
  ```
  * You can use vim: `vim test_Ebuka_Okpala.pbs` to create the file, when vim open type `i` to start typing all the above. When done hit the `esc` button on your keyboard, then hit `shit + :` and type `wq`. The file should be in Dropnet folder. 
  * Or you could simply create this file using you favorite editor and upload it to the Dropnet folder Or create directly in Palmetto [JupyterLab interface](https://www.palmetto.clemson.edu/palmetto/basic/jupyter/) 
  * Don't forget to replace `put_your_username_here` and `put_your_email_here@clemson.edu` with your details 
  * Submit the batch job: `qsub test_Ebuka_Okpala.pbs`
  * You should get an email when the job starts (palmetto assigns you a resource if its available)
  * When the job finishes, you will get an email. The first job should finish in about 2 hours. 
  * Batch jobs don't print outputs until it finishes. If the job finishes successfully you will get an email and the output should be in the file `Ebuka_output_file.txt`. To quickly verify that my code works you might consider running the Jupyter Notebook file - see below. 


**Note**: running the above will only run the first experiment as the rest are commented out. There are 16 experiments that can be ran synchronuously by removing the comments from lines `2061`. This clearly takes time as each experiment will have to finish before the next begins. To cut down on the wait time I ran each experiment one after the other by commenting the rest of the experiments. You can do the same by running each experiment one after the other. To do so, locate the entry point of the program at line `2058` and comment and uncomment the experiment you desire to run. You can remove all comments if you can wait but this takes time, the ResNet and Vgg experiments needs at least 20 hours. 

#### Running the Jupyter Notebook file
- Start by creating a virtual environment
- Open terminal
- shh into your palmetto account 
- Load the anaconda module: `module add anaconda3/2019.10-gcc/8.3.1`
- Create a virtual environment: `conda create --name put_your_env_name_here python=3.8`
- Type yes to accept the installation
- Activate the environment: `source activate put_your_env_name_here`
- Install the packages above:
  - Install tensorflow-gpu: `conda install -c anaconda tensorflow-gpu`
  - The above should have installed numpy
  - There is no need of installing `copy` it comes with Python
  - Install sklearn: `conda install -c anaconda scikit-learn`
  - Install matplotlib: `conda install -c conda-forge matplotlib`
  - For each of the installs, always accept the installations by typing `y` so it does not get killed on palmetto
- Install ipykernel: `conda install ipykernel`
- Create a kernel and give it a name: `python -m ipykernel install --user --name put_your_env_name_here --display-name "tf-gpu (put_your_env_name_here)"`
- Log into the friendly Palmetto [JupyterHub interface](https://www.palmetto.clemson.edu/jhub/hub/home)
- Choose your hadware configuration: `ncpus` 16, `ngpu` 1, `mem` 120gb, `GPU Model` k40, `Interconnect` FDR,  `Walltime` choose enough time maybe 30 minutes for testing purposes. JupyterHub is interactive, if you leave it ideal it disconnects.   
- Open `dropnet_oop.ipynb` 
- Click `kernel` from the top of the page, click `change kernel`, you should see the kernel you created in the terminal e.g `tf-gpu (put_your_env_name_here)`. Select it 
- Run each of the cells 
- Run section 10 - the entry point of the program to start execution
- You should see outputs in a few minutes showing that the experiments is running
- The above note applies here too

#### Outputs
Each of the experiments will automatically create a folder that is named similar to experiment name. The folder contains the results (images/figures)
