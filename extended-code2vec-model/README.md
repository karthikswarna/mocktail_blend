# <div align="center">**Mocktail Model (Extended-code2vec)**</div>

## **Introduction**
* This is an extension to the popular [code2vec](https://github.com/tech-srl/code2vec) model, which learns a distributed representations for code by taking its ASTs paths input. 
* Our model takes paths from multiple graph-based representations, calculates a weighted average (using attention mechanism) of each type of path separately, and aggregates them to create a final code vector. The goal is to use semantic representations like Control Flow Graph (CFG) and Program Dependency Graph (PDG) along with usual ASTs.



## **Setting up the model**
* Please refer the [code2vec](https://github.com/tech-srl/code2vec)'s README to know about installing the required dependencies, model configuration parameters, releasing/exporting the model, exporting the code vectors.
* The settings and parameters specific to our model are mentioned below:

    - **Creating a dataset**

        Use our [Path Extractor](https://github.com/karthikswarna/Path-Extractor-for-C) tool to extract syntactic and semantic paths from C programs and create a dataset.

    - **Training and Evaluating the model**

        - Set the variables in ```run.sh``` to point it to the right dataset. By default, it points to our "testdata_ast_cfg_ddg" dataset, created using the [Path Extractor](https://github.com/karthikswarna/Path-Extractor-for-C) tool.
        
        - Uncomment one of the python commands in ```run.sh``` based on whether you are training the model or evaluating it on the test dataset. 
        
        - Set the ```--reps``` and ```--max_contexts``` arguments in the python commands. Please note that ```--reps``` should be set to the **exact representations included in the dataset**, and ```--max_contexts``` for each type of path must also match the dataset.
            
            - ```--reps``` takes a space-separated list of representations. Allowed values are 'ast', 'cfg', 'cdg', 'ddg'.
            
            - ```--max_contexts``` takes a JSON object where each key is a representation, and its corresponding value is the maximum number of paths for that representation. 

        - You can edit the configuration hyper-parameters in the file ```config.py```, as explained [here](https://github.com/tech-srl/code2vec#configuration).
        
        - Run the ```run.sh``` script:

            ```
            source run.sh
            ```
* Note that we have only implemented the Keras version of the model. Pure TensorFlow implementation is currently not available. 



## **Datasets**
* **Dataset Format**
    - Each training, test, and validation splits should be a single text file, where each row is an example.
    - Each example is a space-separated list of fields, where:
        - The first field is the method name (label), which is separated into subwords by pipe ("|").
        - Each of the following fields is a set of AST path-contexts followed by CFG path-contexts and then PDG path-contexts. The only way to differentiate a path AST or CFG or PDG path is by counting its position from the start (This is why it is important to set the correct value for ```--max-contexts``` argument.) If an example has fewer path-contexts than ```--max-contexts```, it should be padded with additional spaces to compensate for fewer paths.
        - Each context has three components separated by commas (","). Each of these components cannot include spaces nor commas. The first and third components of a path context are the context words, and the second component is the actual path.

* The datasets we have created for our project can be found [here](https://drive.google.com/file/d/1CmQSOVvoR8zObc-Rbh8Un5d3dNFvCfbF/view?usp=sharing).


## **Testing the model**
* You can test if all the dependencies are properly installed by training the model on the ```testdata``` we have provided. Before training just set the following parameteres in the ```config.py``` file.
        
        self.TRAIN_BATCH_SIZE = 2
        self.TOP_K_WORDS_CONSIDERED_DURING_PREDICTION = 2

## **Team**
In case of any queries or if you would like to give any suggestions, please feel free to contact:

- Karthik Chandra (cs17b026@iittp.ac.in) 

- Dheeraj Vagavolu (cs17b028@iittp.ac.in) 

- Sridhar Chimalakonda (ch@iittp.ac.in)