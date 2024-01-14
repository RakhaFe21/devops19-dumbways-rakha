# Day 8 (Challenge Task)
Challenge Task:

1.  Dengan mendaftar akun free tier AWS/GCP/Azure, buatlah Infrastructre dengan terraform menggunakan registry yang sudah ada. (spec menyesuaikan saja dengan free tier yang di dapatkan)

## Step by step
### Mendaftar akun microsoft azure
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2010-14-50.png?raw=true)
### Terraform environment
Menggunakan dokumentasi resmi microsoft azure :
https://learn-microsoft-com.translate.goog/en-us/azure/virtual-machines/linux/quick-create-terraform?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc&tabs=azure-cli 

1. Membuat file `providers.tf`
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2851%29.png?raw=true)
2. Membuat `ssh.tf`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2852%29.png?raw=true)
3. Membuat file `main.tf`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2853%29.png?raw=true)
4. Membuat `variables.tf`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2854%29.png?raw=true)
5. Membuat `output.tf`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2855%29.png?raw=true)
### Excecution
1. Initialize Terraform
	`terraform init -upgrade`
2. Create a Terraform execution plan
`terraform plan`
3. Apply a Terraform execution plan
`terraform apply`

### Result
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2010-44-00.png?raw=true)
