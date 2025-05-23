### Description of how to import a 3d model into Unity  

![Import Npc](https://i.postimg.cc/FzY98kZV/1.png)  

My Unity client does not have Fishing Guild Member Klufe NPC, now we want to add it to the client:  

1. We find the file Npcgrp.dat, read it using FileEditor and find our NPC his ID: 31562  
In the file we find the line with these IDs and cut out the following parameters:  
mesh_name=LineageNPSs.a_fisher_Fog_m00  
texture_name:
_1 LineageNPCsTex.a_fisherA_Mhuman_m00_t00_b00
_2 LineageNPCsTex.a_fisherA_Mhuman_m00_t00_b01
_3 LineageNPCsTex.a_fisherA_Mhuman_m00_t00_f
_4 LineageNPCsTex.a_fisherA_Mhuman_m00_t00_h


From these parameters we got everything we need: the name of the mash file and the names of its textures.  

2. Now we need to find these files:  
We start the search for the original client:  
_1 Animations\LineageNpcs.ukx (this is the folder with the necessary files)   
_2 Launch the Gildor archiver and get the finished files:  
_3 LineageNpcs\SkeletalMesh\a_fisherA_Mhuman_m00.psk this is the NPC mesh  
_4 Open Blender with the installed plugin for reading psk files  
_5 Import this file and Export it to the format a_fisherA_Mhuman_m00.fbx  
_6 Return to belder and click import .psa files; this file contains animations for our NPC  
_7 LineageNpcs\MeshAnimation\a_fisherA_MHuman_anim.psa  

3. We found Mash, now we need to pull out the textures for our NPC  
_1 Find LineageNPCsTex this folder in the client     
_2 Launch the Gildor archiver and get the finished files:    
_3 I found all the files in the LineageNpcsTex\Texture\ folder  
_4 I copy all received files into a separate folder so that I can quickly find them in the future.  


4. Now we open Unity L2  
_1 Go to the folder Assets\Resources\Data\Animations\LineageNPCs  
_2 Create a new folder named a_fisherA_Mhuman_m00  
_3 Create a new folder named Model    
_4 Drag a_fisherA_Mhuman_m00.fbx to the Model folder  
_5 Drag the appeared file into a blue scene to create a ready-made prefab with the same name but prefab extension  

![Import Npc](https://i.postimg.cc/ZYQY3jb7/2.png)  


_6 double-click on the prefab and see this picture  

![Settings](https://i.postimg.cc/Wzx2vpYL/3.png)  


_7 The height of the Npc is set incorrectly. We need to fix this by going to the CharacterController:  

![The height of the Npc](https://i.postimg.cc/vH6Yv9DQ/4.png)  

 

_8 Now we need to add textures to this mesh, take 4 of our texture files and drag them into the folder  
 Assets\Resources\Data\SysTextures\  
_9 After copying, go to the folder Assets\Resources\Data\SysTextures\Materials  
and create 1 material a_fisherA_MHuman_m00_t00_b00  
_10 Left-click on a_fisherA_MHuman_m00_t00_b00.mat and drag a texture with the same name a_fisherA_MHuman_m00_t00_b00.png into the base map  
 repeat this 4 times for all textures  
 _11 Create an empty object inside prefab GameObject->CreateEmpty and rename it click_area  
 _12 Select click_area and add components: Cube Mesh Filter, BoxCollider  
 _13 Select Layer->EntityClick  
 _14 In BoxCollider we set Scale 0.25 0.9 0.25  
 _15 You can start the game and watch our npc (we will also need to connect the animator to this npc, but that’s for another time!)  
