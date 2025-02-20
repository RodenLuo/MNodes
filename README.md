# Molecular Nodes 🧬🔬💻

### Buy Me a Coffee to Keep Development Going!
<a href="https://www.buymeacoffee.com/bradyajohnston" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-violet.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

### Join a Community of Blender SciVis People!
<a href="https://discord.gg/hbRp2NYDqK"><img src="https://discord.com/api/guilds/940526858800336936/widget.png?style=banner1"></a>


### What is Molecular Nodes?
Molecular Nodes provides a convenient method for importing structural biology files into Blender, and several nodes for working with atomic data inside of Blender's Geometry Nodes.

Blender's Geometry Nodes provides a powerful interface for procedural modelling and animation. Currently it is limited in its ability to read any kind of structured data file as input, that isn't a 3D mesh. Molecular Nodes bridges this gap by providing an interface for converting `.pdb` and other file types into meshes that are usable by Geometry Nodes.

![](img/atp-animation-demo.gif)

## Acknowledgements
This addon is built crucially around the python library [`atomium`](https://github.com/samirelanduk/atomium), which provides a great lightweight and pure-python way to to parse a variety of structural biology files, that interface nicely with the built-in python interpreter that is bundled with Blender.

This addon is built using the _excellent_ plugin for creating addons [Serpens](https://joshuaknauber.notion.site/Serpens-Documentation-d44c98df6af64d7c9a7925020af11233), which made the process of compiling a clear and concise addon, significantly reducing development time an creating a better addon overall.

Importantly none of this would be possible without the [Blender](https://www.blender.org/about/foundation/) and the [Blender Foundation](https://www.blender.org/about/foundation/) and the fantastic work of all the developers, from the community and the foundation itself. 

> ### This documentation is currently not complete for all of the nodes, but there should be enough to get you started. More complete documentation is in the works.

## Installation

> This addon is built with Blender v. 3.1.2, some of the nodes may break in earlier versions.

To install Molecular Nodes, download the [latest release](https://github.com/BradyAJohnston/MolecularNodes/releases) and install the addon through the preferences panel inside of Blender, and select the `MolecularNodes_0.3.8.zip` file
> Edit -> Preferences -> Addons -> Install
![Screenshot highlighting the install addon button.](img/install-addon.png)

If the addon isn't highlighted, search for it and click the tick box to enable the addon.
![Screenshot highlighting the enable Molecular Nodes Addon](img/enable-addon.png)

### Installing Atomium
> Windows: You must run Blender as Administrator to be able to install Atomium successfully.

![Screenshot highlighting the Install Atomium Button](img/install-atomium.png)

While still in the preferences panel, click the `Install Atomium` button to download and install the [Atomium python library](https://github.com/samirelanduk/atomium). This should only need to be done the first time you install Molecular Nodes. You will need to complete this step again if you reinstall Blender or upgrade the version.

The addon should now be enabled and available for use. 

## Loading Structures
You can load structures using the Molecular Nodes panel, which should be available at the top-right of the 3D viewport. You can press `N` to show & hide the side panels. Click on the `Molecular Nodes` tab to reveal the panel.


![Screenshot highlighting the Molecular Nodes Penal](img/mol-panel-viewport.png)

Molecular Nodes provides two ways to open files. You can fetch directly from the PDB by inputting the 4-character code into the top box and pressing the download button. This will download the file and open it inside of Blender.


![Screenshot showing the interface of the Molecular Nodes Panel](https://user-images.githubusercontent.com/36021261/165198468-84e813b5-e5e6-4cfb-b735-cc78ae45791f.png)


You can also open a file saved on your local computer. Click the folder icon to navigate to your `.pdb` file, select it and press OK. Then press `Open` on the Molecular Nodes panel to read the file and load the model into Blender.

## Using the Nodes

### Atomic Properties
With Molecular Nodes enabled, create a geometry nodes node tree, and inside the add menu (`Shift + A`) there will be an additional category for Molecular Nodes. 
![image](https://user-images.githubusercontent.com/36021261/165195736-def01745-aa71-456a-956d-0cc40bc43df8.png)

To access the atomic properties that are associated with the model, add the "Atomic Properties" node from `Properties -> Atomic Properties` and in the node, select the `*_properties` collection. This will scale the points according to their Atomic Radii, and make all of the atomic properties associated with the atoms (atomic number, is_backbone / is_sidechain) available for use inside of the node tree.

### Styling
![image](https://user-images.githubusercontent.com/36021261/165195935-9cda165c-6b05-4c14-a9c9-12748ad006b7.png)

To quickly colour the structure, add the "Style Colour" node from `Styling -> Style Colour`, select a material for the atoms, and connect the `atomic_number` from the `Properties` node to the `atomic_number` of the style node. The outputted colour becomes an output for the node tree.

![image](https://user-images.githubusercontent.com/36021261/165196242-038cd2d7-5270-426a-9c13-a3a9ef43ac57.png)

Name the output for the node tree in the modifier tab: 
![image](https://user-images.githubusercontent.com/36021261/165196280-bcd62e5a-3dcc-4f52-aede-a4227f8c1046.png)

And add an attribute node to the shader, inputting the name for the colour output fromt the GN node tree, in this case `colour`. 
![image](https://user-images.githubusercontent.com/36021261/165196355-c58de115-13c6-4533-9bad-e0d86806e7b7.png)

### Voila!
> By default, the atoms are 'point clouds' and only visible inside of Cycles. 

The atoms should now be visible, inside of the Cycles render engine: 
![image](https://user-images.githubusercontent.com/36021261/165196439-1fa247f3-cf44-40d1-a13a-57320c341f3a.png)

To make the atoms visible inside of EEVEE, use the `Styling -> Atoms EEVEE` node. You will need to create and assign a material for each element individually.

![image](https://user-images.githubusercontent.com/36021261/165196698-2abe212a-9175-4c50-89a0-313c75e3bc31.png)

## Animating Frames
To animate between frames of a `.pdb` file, add the `Animate Frames` node from `Animation -> Animate Frames` and choose the `*_frames` collection to give the model the infomation of the different structures. You can now animate between the frames by using the `Animate 0...1` slider which will animate between the first frame (`0`) and the last frame (`1`). Attach the `Animate Node` from `Animation -> Animate` to automatically play through the frames.

![image](https://user-images.githubusercontent.com/36021261/165197211-8b806065-5d69-4b49-abab-1fd425b48c77.png)

You can now play back the animation!

In the example below, I create the `.pdb` frames from [this tweet](https://twitter.com/UCSFChimeraX/status/1258888093068701696?s=20&t=zDVE14P-Q6HfHJtnJpsKSw) and imported them with the steps above, and added a `Random Vector` based on the chain number, to colour the carbons of the different chains different colours.

Inside of [ChimeraX](https://www.cgl.ucsf.edu/chimerax/) run these three commands:
1. `open 6n2y 6n2z 6n30`
2. `morph #1,2,3 wrap true`
3. `save atp-frames.pdb #4 allCoordsets true`

And then open `atp-frames.pdb` inside of Molecular Nodes.

![](img/atp-animation-demo.gif)

Want to support the development of this addon? Buy me a coffee! (it took a lot of them to make it)

<a href="https://www.buymeacoffee.com/bradyajohnston" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-violet.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>



