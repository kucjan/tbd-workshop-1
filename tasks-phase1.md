IMPORTANT ❗ ❗ ❗ Please remember to destroy all the resources after each work session. You can recreate infrastructure by creating new PR and merging it to master.
  
![img.png](doc/figures/destroy.png)

1. Authors:

   ***15***

   https://github.com/kucjan/tbd-workshop-1
   
2. Follow all steps in README.md.

3. Select your project and set budget alerts on 5%, 25%, 50%, 80% of 50$ (in cloud console -> billing -> budget & alerts -> create buget; unclick discounts and promotions&others while creating budget).

  ![img.png](doc/figures/discounts.png)

5. From avaialble Github Actions select and run destroy on main branch.

![Screenshot 2024-12-02 at 20 00 53](https://github.com/user-attachments/assets/c36967fe-7f0a-4188-a62c-58a0059da6e8)
   
7. Create new git branch and:
    1. Modify tasks-phase1.md file.s
    
    2. Create PR from this branch to **YOUR** master and merge it to make new release. 
    
    ![Screenshot 2024-12-02 at 20 05 15](https://github.com/user-attachments/assets/779f81bf-0cc4-4813-aec4-8256ff5395ee)



8. Analyze terraform code. Play with terraform plan, terraform graph to investigate different modules.
  
    Module: vertex-ai-workbench
    
    Description
    This module is designed to set up a Virtual Machine (VM) with a pre-configured disk image that includes all the tools required for interacting with infrastructure supporting Big Data analysis. Using this VM is more convenient and efficient than configuring a local private machine to access necessary cloud resources. Additionally, the module provides a user-friendly web interface for seamless interaction.
    
    Resources
    
    - **google_notebooks_instance** – Provisions and configures the virtual machine.  
    - **google_project_service** – Enables project-level access to Google services, specifically the Notebooks API in this case.  
    - **google_project_iam_binding** – Assigns a set of permissions (role) to a service account, allowing it to generate short-lived credentials needed for authenticating Google services from Jupyter notebooks.  
    - **google_storage_bucket** – Creates a storage bucket, which is the simplest form of file storage in Google Cloud Storage (GCS).  
    - **google_storage_bucket_iam_binding** – Grants read-only access to the storage bucket for a specific service account. This access is necessary to retrieve the post-startup script.  
    - **google_storage_bucket_object** – Uploads the post-startup configuration script to the GCS bucket during the `terraform apply` process.


   Graph:

    ```
    cd modules/vertex-ai-workbench
    terraform init -upgrade
    terraform graph -type=plan | dot -Tpng > terraform-plan-graph-vertex-ai-workbench.png
    ```

    ![terraform-vertex](https://github.com/user-attachments/assets/13e7c28c-551e-4388-bd25-147938e5f2f7)

   
9. Reach YARN UI
   
   COMMAND:

    ```bash
    # Create an SSH tunnel using local port 1080
    gcloud compute ssh tbd-cluster-m \
    --project=tbd-2024z-303753 \
    --zone=europe-west1-d -- -D 1080 -N

    # Run Chrome and connect through the proxy (macOS)
    /usr/bin/google-chrome --proxy-server="socks5://localhost:1080" \
    --user-data-dir="/tmp/tbd-cluster-m" http://tbd-cluster-m:8088
    ```

    ![Screenshot 2024-12-02 at 21 52 15](https://github.com/user-attachments/assets/f8fd13ff-5e88-4665-88b2-1be8083d91ed)

   
10. Draw an architecture diagram (e.g. in draw.io) that includes:
    1. VPC topology with service assignment to subnets
    2. Description of the components of service accounts
    3. List of buckets for disposal
    4. Description of network communication (ports, why it is necessary to specify the host for the driver) of Apache Spark running from Vertex AI Workbech
  
    ***place your diagram here***

11. Create a new PR and add costs by entering the expected consumption into Infracost
For all the resources of type: `google_artifact_registry`, `google_storage_bucket`, `google_service_networking_connection`
create a sample usage profiles and add it to the Infracost task in CI/CD pipeline. Usage file [example](https://github.com/infracost/infracost/blob/master/infracost-usage-example.yml) 


   ***place the expected consumption you entered here***

   ***place the screenshot from infracost output here***

11. Create a BigQuery dataset and an external table using SQL
    
    ***place the code and output here***
   
    ***why does ORC not require a table schema?***

  
12. Start an interactive session from Vertex AI workbench:

  COMMAND:

  ```bash
  gcloud compute --project "tbd-2024z-303753" ssh --zone "europe-west1-b" "tbd-2024z-303753 notebook" -- -L 8080:localhost:8080
  ```

  ![Screenshot 2024-12-02 at 22 10 59](https://github.com/user-attachments/assets/69301af6-4167-4629-9e4d-83543c82a2a7)

   
13. Find and correct the error in spark-job.py

    ***describe the cause and how to find the error***

14. Additional tasks using Terraform:

    1. Add support for arbitrary machine types and worker nodes for a Dataproc cluster and JupyterLab instance

    ***place the link to the modified file and inserted terraform code***
    
    3. Add support for preemptible/spot instances in a Dataproc cluster

    ***place the link to the modified file and inserted terraform code***
    
    3. Perform additional hardening of Jupyterlab environment, i.e. disable sudo access and enable secure boot
    
    ***place the link to the modified file and inserted terraform code***

    4. (Optional) Get access to Apache Spark WebUI

    ***place the link to the modified file and inserted terraform code***
