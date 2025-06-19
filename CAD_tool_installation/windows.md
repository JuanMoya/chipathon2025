# Installation Instructions for Windows - More Advanced Workflow

Juan Moya, June 2025. 

## Preamble

These instructions help you install a more advanced set of s/w components to work with the open-source IC design tools. Beyond *Docker* and *VNC*, these instructions also use *VS Code* and *MobaXterm*. 

## Installation Steps

The Docker image used in this 2025 Chipathon is pre-packaged Docker container provided by Harald Pretl's lab **IIC-OSIC-TOOLS**: https://github.com/iic-jku/iic-osic-tools

First, a block diagram with the main installation steps:
<p align="center">
   <img src="./img/Installation_flow.png" width="600" />
</p>  

GUI-based tool setup:
1) Download Docker: https://www.docker.com/get-started/

  a) Enable *Use WSL 2 instead of Hyper-V (recommended)* as indicated in Kwantae Kim's tutorial: https://kwantaekim.github.io/2024/05/25/OSE-Docker/#in--windows

2) Search and pull Harald Pretl's container in Docker app: *iic-osic-tools* (it will take a couple of minutes).

3) Download and install Windows Subsystem for Linux (WSL).
   
a) Open WSL and run:
```
   wsl -l -v
```
4) In the Docker app, enable integration with Ubuntu.

<p align="center">
   <img src="./img/Ubuntu_WSL_integration.png" width="600" />
</p> 

6) Download and install VS Code: https://code.visualstudio.com/

7) Install Docker extension in VS Code

8) Download and install MobaXterm: https://mobaxterm.mobatek.net/

   MobaXterm allows users to run graphical applications on a remote server and display them locally, typically over an encrypted SSH connection.

9) Open VS Code, open the terminal and **<ins>open a new  Ubuntu WSL terminal</ins>**.

  a) Create a folder where you will pull Harald Pretl's Docker image.

  b) Clone Harald Pretl's Docker image from: https://github.com/iic-jku/iic-osic-tools with the following command:
  ```
  git clone https://github.com/iic-jku/IIC-OSIC-TOOLS.git
  ```

9) To allow us to run Docker by using WSL terminal, run the following line in the WSL terminal:
  ```
  sudo chmod 666 /var/run/docker.sock
  ```

10) Execute the following command in **Powershell terminal** in the IIC-OSIC-TOOLS folder:
  ```
  .\start_vnc.bat
  ```

11) Building X11 Forwarding by installing VS Code Server for Linux x64. Run the following command in the IIC-OSIC-TOOLS folder:
   ```
  code .
  ```

12) Modify line 63 of <ins>start_vnc.bat</ins>, and include the following:
  ```
  SET PARAMS=%PARAMS% -e DISPLAY=host.docker.internal:0
  ```

13) Modify line 23 of <ins>start_vnc.bat</ins>, and include the following:
  ```
  SET DEFAULT_DESIGNS=C:/mnt/c/Users/{your username}/{path where IIC-OSIC-TOOLS folder was cloned}
  ```
14) Delete Docker container in the app.

15) Run again the following command in **Powershell terminal** in the IIC-OSIC-TOOLS folder:
  ```
  .\start_vnc.bat
  ```

16) A new terminal pops up.
<br>
:arrows_clockwise::arrows_clockwise::arrows_clockwise: Switching between PDKs :arrows_clockwise::arrows_clockwise::arrows_clockwise:

When you open the simulation environment, it is possible that the PDK set is different from the expected one for the 2025 Chipathon(Global Foundries 180nm). If this is the case, follow the next steps to switch PDKs.
 a) In the terminal that popped up, type the following command:
   ```
  cd foss/designs
  ```

 b) Then, type the next command:
  ```
  gedit setup_pdk.sh
  ```

c) Modify the setup_pdk.sh file, based on the table beelow, as illustrated in the next figure and then save it:

<p align="center">
   <img src="./img/setup_pdk.png" width="600" />
</p>  

d) Now, we need to make the file executable:
  ```
  chmod +x setup_pdk.sh
  ```

e) Now source the file to be able to work with Global Foundries 180nm PDK:
  ```
  source setup_pdk.sh
  ```

| PDK | SkyWater 130nm | Global Foundries 180nm |  ihp sg13g2 130nm |
| ----------- | ---- | --------- | ----- |
| PDK Library |	export PDK = sky130A | export PDK = gf180mcuC | export PDK=ihp-sg13g2 |
| Path to the PDK Library |	export PDKPATH = $PDK_ROOT/$PDK | export PDKPATH = $PDK_ROOT/$PDK	| PDKPATH=$PDK_ROOT/$PDK |
| Standard Cell PDK Library |	export STD_CELL_LIBRARY = sky130_fd_sc_hd | export STD_CELL_LIBRARY = gf180mcu_fd_sc_mcu7t5v0	| sg13g2_stdcell |

<br>
<br>
<br>

:rotating_light: :rotating_light: :rotating_light: :rotating_light: Important information: :rotating_light: :rotating_light: :rotating_light: :rotating_light: 

Once the Docker image has been installed correctly, you need to follow these steps each time you access it locally:

- Open Docker app
- Open Mobaxterm and run X server (X11 Forwarding)
- Run in **Powershell terminal** the command:
  ```
  .\start_vnc.bat
  ```
  - Source in the popped-up terminal the next command to set Global Foundries 180nm PDK:
  ```
  source setup_pdk.sh
  ```
