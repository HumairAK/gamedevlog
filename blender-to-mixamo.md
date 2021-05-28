We are going to take the following character from Blender and get some animations for it from Mixamo.

![image](https://user-images.githubusercontent.com/10904967/119897415-5ddacd80-bf0e-11eb-8cae-0a1a5bc4e87f.png)

1. Select the objects, and export as OBJ, use default settings but check "Selection Only"
2. Import the OBJ into [mixamo](https://www.mixamo.com):
  - Upload the character
  - Apply joints
  - Select the appropriate animation
4. Click download, for the first object do it with skin (to get the weights) 
![image](https://user-images.githubusercontent.com/10904967/119897926-1b65c080-bf0f-11eb-976c-ac5030465c14.png)

5. Import the downloaded fbx into Blender (with 100 scale)
![image](https://user-images.githubusercontent.com/10904967/119898024-3fc19d00-bf0f-11eb-9db4-00e24fb98e05.png)

I also check "Apply Transform" as it yields better results when trying to use the same armature for a different imported animation. The imported objects might look funky, but we won't be using these so I think it's not an issue. (If anyone knows why this happens please let me know, I still haven't figured it out).

### Hook up Armature to existing objects

So what I want is the animation and vertex groups/weights. For the first animation I import, I keep the armature, but for the successive imports I'll clear their armatures.

I like to use my existing objects, this requires copying the vertex groups over from the imported armature.

![image](https://user-images.githubusercontent.com/10904967/119919663-e706fa00-bf38-11eb-939a-015547ad737a.png)

In the image above you scan see the imported armature has their own instances of the original objects. I'll refer to the originals as OB (original objects) and armature's child objects as AB (armature objects). 

First add the modifier `Armature` to the OB's. Select the imported Armature for the `object` field in the modifier: 

![image](https://user-images.githubusercontent.com/10904967/119919804-31887680-bf39-11eb-895e-90988241da2b.png)

Do this for all OB's. (Do this quickly by selecting all OB's and pressing ctrl+l in the viewport and selecting `modifiers`). 

Next, for each OB one by one do the following: 

  - Select OB
  - Select the matching AB (the selecting order is important)
  - go to vertex groups and select the arrow on the right
  - pick copy vertex groups to selected

![image](https://user-images.githubusercontent.com/10904967/119920126-c7bc9c80-bf39-11eb-9c0e-2c9cfcd9c804.png)

Once done delete the objects under the armature.

### Add another animation

For our game character we probably wan't another animation. 

Download another animation from Mixamo, I've found it a bit bugging when importing "without skin", so I just keep the skin (but remove it later since we use our existing objects). 

Then import it just like before. This time you can delete this new Armature and it's child objects, keeping the animation.

Select the Dope Sheet > Action Editor panel, and rename the animations accordingly.

![image](https://user-images.githubusercontent.com/10904967/119923180-3e0fcd80-bf3f-11eb-9292-e2b878f38c69.png)

