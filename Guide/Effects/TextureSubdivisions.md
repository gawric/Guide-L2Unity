### A theory on how to create a sprite atlas in UC files

![img1](https://i.ibb.co/5hDLn1xY/Sprite-Sub-Devision.png)    

         TextureUSubdivisions=2
         TextureVSubdivisions=4
         UseRandomSubdivision=True
         SubdivisionStart=2
         SubdivisionEnd=4

In the effect file configuration, we see     
TextureUSubdivisions=2 > 2 split into 2 columns,  
TextureVSubdivisions=4, and 4 rows.      
    
SubdivisionStart=2 Starts at index 2  
SubdivisionEnd=4 Ends at index 3  