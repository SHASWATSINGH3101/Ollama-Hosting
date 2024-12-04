
![image (3)](https://github.com/user-attachments/assets/7b60a31a-b839-4d9d-be02-c4ce27b7fe5b)



# Ollama-Hosting
This project shows how to run Ollama on a weak computer using Kaggle for free GPU hosting, Ngrok for secure tunneling, and Open Web UI for interaction. It’s a cost-effective, accessible solution for developers and researchers to manage AI models privately without relying on expensive hardware.

## Installation

-> Go this website [Ollama](https://ollama.com/), download Ollama and install it in your system.


-> Install Open WebUI: Open your terminal and run the following command to install Open WebUI:
```
pip install open-webui
```
Running Open WebUI: After installation, you can start Open WebUI by executing:
```
open-webui serve
```
## Kaggle Setup

Use this [notebook](https://github.com/SHASWATSINGH3101/Ollama-Hosting/blob/main/code/ollama-server_KAGGLE.ipynb) to start the Ollama and Ngrok.
Make sure to add your Ngrok AUTH-token:
```
"your-authtoken"
```

## Ollama setup

  1. Ngrok exposes a url, which you the have to set as OLLAMA_HOST after that we can use ollama on our remote instance from our local machine.
        ```
        set OLLAMA_HOST=https://fd90-34-125-15-193.ngrok.io/   (example URL)
        ```
  2. Run a test model
       ```
       ollama run llama3.2:1b
       ```
  3. Check the logs for model downloading , and you can use ollama from your Terminal now.

## Open-webui setup

  1. Ngrok exposes a url, which you the have to set as OLLAMA_BASE_URL after that we can connect to ollama.
     
        ```
        set OLLAMA_BASE_URL=https://fd90-34-125-15-193.ngrok.io/   (example URL)
        ``` 
  3. Run open-webui
     
       ```
       open-webui serve
       ```
This is from where i got the initial code [Credit](https://github.com/marcogreiveldinger/videos/blob/main/ollama-ai/run-on-colab/ollama-ai-colab.ipynb)

Thanks @marcogreiveldinger

## Model upload to Kagglehub

  This code lets you upload the model to kagglehub so that you can re-use your downloaded models and save some time, adjust details as per the need:

  ```python
      # Step 1: List the contents of the Ollama models directory
    import os
    print("Listing the contents of /root/.ollama/models/")
    print(os.listdir("/root/.ollama/models/"))
    
    # Step 2: Create the target directory on Kaggle's working environment
    print("Creating the directory /kaggle/working/ollama_models/")
    os.makedirs("/kaggle/working/ollama_models", exist_ok=True)
    
    # Step 3: Copy the contents from the Ollama model directory to the target directory
    import shutil
    print("Copying models to /kaggle/working/ollama_models/")
    shutil.copytree("/root/.ollama/models", "/kaggle/working/ollama_models", dirs_exist_ok=True)
    
    # Step 4: Install Kaggle Hub
    print("Installing kagglehub package")
    !pip install kagglehub
    
    # Step 5: Set Kaggle username and key for authentication
    import os
    os.environ["KAGGLE_USERNAME"] = ""  # Replace with your actual Kaggle username
    os.environ["KAGGLE_KEY"] = ""  # Replace with your actual Kaggle API key
    
    # Step 6: Upload model to Kaggle Hub
    import kagglehub
    
    # Set your model handle and local model directory path
    handle = "KAGGLE_USERNAME/model-name/package/version"  # Replace with your model handle
    local_model_dir = "/kaggle/working/ollama_models/"
    
    # Upload the model with optional version notes
    kagglehub.model_upload(handle, local_model_dir, version_notes="Upload date :- 02/12/2024")
    
    # Optional: You can also specify a license
    # kagglehub.model_upload(handle, local_model_dir, license_name="Apache 2.0")
    
    # Optional: Specify patterns to ignore during upload
    # kagglehub.model_upload(handle, local_model_dir, ignore_patterns=["original/", "*.tmp"])
    
    print("Model upload complete!")

```

## Soft-linking thte model

  This code lets you Soft-link your saved models on the kagglehub, so you can reuse them:

  ```python
    
        import os
        import shutil
        
        def sync_or_copy(source_dir, target_dir, folder_name):
            """
            Handle syncing or copying a folder based on its existence in the target directory.
            
            If the filesystem is read-only, it will skip the write operation.
            
            Parameters:
            - source_dir (str): Path to the source directory containing the folder to copy.
            - target_dir (str): Path to the target directory where the folder should be copied or synced.
            - folder_name (str): Name of the folder to process (e.g., 'manifests', 'blobs').
            """
            source_folder = os.path.join(source_dir, folder_name)
            target_folder = os.path.join(target_dir, folder_name)
        
            if os.path.exists(target_folder):
                print(f"'{folder_name}' folder exists at {target_folder}. Copying contents...")
                for item in os.listdir(source_folder):
                    src_item = os.path.join(source_folder, item)
                    dest_item = os.path.join(target_folder, item)
                    
                    if os.path.isdir(src_item):
                        if os.path.exists(dest_item):
                            print(f"Directory exists, merging: {dest_item}")
                        try:
                            shutil.copytree(src_item, dest_item, dirs_exist_ok=True)
                        except OSError as e:
                            print(f"Skipping {src_item}: {e}")
                    else:
                        try:
                            shutil.copy2(src_item, dest_item)
                        except OSError as e:
                            print(f"Skipping {src_item}: {e}")
            else:
                print(f"'{folder_name}' folder does not exist at {target_dir}. Copying the entire folder...")
                try:
                    shutil.copytree(source_folder, target_folder)
                except OSError as e:
                    print(f"Error copying {folder_name}: {e}")
        
        # Define source and target paths
        source_dir = "/kaggle/input/lama3_2_1b/pytorch/v1/3"
        target_dir = "/root/.ollama/models"
        
        # Process the manifests and blobs folders
        sync_or_copy(source_dir, target_dir, "manifests")
        sync_or_copy(source_dir, target_dir, "blobs")
```

## GGUF conversion

  This [notebook](https://github.com/SHASWATSINGH3101/Ollama-Hosting/blob/main/code/GGUF%20converts%20.ipynb) lets you convert model into GGUF format , so that you can use custom models and     huggingface models.

  
  Example command to a huggingface model
  ```
  ollama run hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF
  ```



Connect with me [here](https://www.linkedin.com/in/shaswat-singh-43821826a/)


```bibtex
@software{Ollama-Hosting,
  author  = {SHASWATSINGH3101},
  title   = {Ollama-Hosting},
  url     = {https://github.com/SHASWATSINGH3101/Ollama-Hosting}
  year    = 2024,
  month   = December
}
```

## License

[Apache 2.0 license:](https://www.apache.org/licenses/LICENSE-2.0)
