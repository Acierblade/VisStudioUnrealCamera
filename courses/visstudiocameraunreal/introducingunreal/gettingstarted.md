---
layout: page
title: "Setting Up a Project for the Visualization Studio"
course: "visstudiocameraunreal"
unit: 1
---
{% include youtube.html id="ewRKuI49vPQ" title="Visualization Studio Unreal Engine Setup Walkthrough" %}

---

#### <a href="codesnippets.html" target="_blank">Code Snippets</a>

---
<div style="height: 500px; overflow: scroll;">
<h4>Jump To:</h4>
<p1>
<a href="#[00:00]">[00:00]</a><br>
<a href="#[01:16]">[01:16]</a><br>
<a href="#[02:18]">[02:18]</a><br>
<a href="#[02:44]">[02:44]</a><br>
<a href="#[02:57]">[02:57]</a><br>
<a href="#[03:49]">[03:49]</a><br>
<a href="#[10:53]">[10:53]</a><br>
<a href="#[11:29]">[11:29]</a><br>
<a href="#[11:43]">[11:43]</a><br>
<a href="#[12:53]">[12:53]</a><br>
<a href="#[14:08]">[14:08]</a><br>
<a href="#[18:29]">[18:29]</a><br>
<a href="#[24:37]">[24:37]</a><br>
<a href="#[29:57]">[29:57]</a><br>
<a href="#[30:40]">[30:40]</a><br>
<a href="#[31:13]">[31:13]</a><br>
<a href="#[31:55]">[31:55]</a><br><br>
</p1>

<p id="[00:00]"><h4>[00:00]</h4>

Begin with a C++ Unreal Project (if using an existing Blueprint-only project, it must first be converted to a C++ project).
<br><br>
In the content browser, navigate to C++ Classes, and then into the folder with your project’s name.
<br><br>
Create a new C++ class with Actor as the Parent Class. 
<br><br>
Name this <b>“ASplitScreenManager”</b>
<br><br>
Copy all of the provided code for ASplitScreenManager.cpp, and replace the default code with it.
<br><br>
For ASplitScreenManager.h copy ONLY the line <em>void ApplyEightViewportSettings();</em> and insert that line directly under <em>AASplitScreenManager();</em>
<br><br>
Save the files, and return to the editor.
<br><br>
</p>

<p id="[01:16]"><h4>[01:16]</h4>

Create another C++ class, being sure to check “Show All Classes”
<br><br>
Search for and select GameInstance as the parent class.
<br><br>
Name this <b>“VizStudioGameInstance”</b>
<br><br>
Like before, copy all of the provided code for VizStudioGameInstance.cpp, and replace the default code.
<br><br>
For VizStudioGameInstance.h, only the lines from  <em>public:</em>  to <em>ULocalPlayer…</em>  need to be copied from the provided code and pasted just below <em>GENERATED_BODY()</em> in Visual Studio.
<br><br>
</p>

<p id="[02:18]"><h4>[02:18]</h4>

Back in the editor, create one more C++ class, and select GameViewportClient as the parent class.
<br><br>
Name this <b>“VizStudioGameViewportClient”</b>
<br><br>
This requires no additional code.
<br><br>
</p>

<p id="[02:44]"><h4>[02:44]</h4>

Now create a Blueprint class from ASplitScreenManager.
<br><br>
Name this <b>“ASplitScreenManagerBP”</b>
<br><br>
This should be placed in your project’s Content folder. Go there now.
<br><br>
</p>

<p id="[02:57]"><h4>[02:57]</h4>

Now to create a few more Blueprint classes.
<br><br>
Create a Pawn class, and name it <b>“VizCameraReceiverPawn”</b>
<br><br>
Create a Game Mode Base class, and name it <b>“VizGameMode”</b>
<br><br>
Create another Pawn class, and name it <b>“VizParentPawn”</b>
<br><br>
Finally, create a Player Controller class, and name it <b>“VizPlayerController”</b>
<br><br>
</p>

<p id="[03:49]"><h4>[03:49]</h4>

Open up the VizGameMode Blueprint.
<br><br>
Follow along with the video to continue setting up the VizGameMode.
<br><br>
</p>

<p id="[10:53]"><h4>[10:53]</h4>

At this point, be sure that the Possess node inside your collapsed Change First Player To A Parent Pawn node has an execute output running to the collapsed node’s Outputs node.
<br><em>
Even if the collapsed nodes appear to be connected from the Event Graph, they will not be if this change is not made.</em>
<br><br>
From here, continue following along with the video to set up Panini Projection in the Construction Script.
<br><br>
</p>

<p id="[11:29]"><h4>[11:29]</h4>

Go to VizGameMode’s Class Defaults, and change the Default Pawn Class to <b>VizCameraReceiverPawn</b>, <em>NOT VizParentPawn</em>
<br><br>
Set the HUD class to None
<br><br>
With that, the VizGameMode Blueprint is set up.
<br><br>
</p>

<p id="[11:43]"><h4>[11:43]</h4>

<em>*Technically, this section is optional if you do not plan to move the player in your project, or if you simply do not want vignetting when moving. If that is the case, skip to [14:08].</em>
<br><br>
Now, make a new material named Blur_PP.
<br><br>
Follow the video closely for steps.
<br><br>
</p>

<p id="[12:53]"><h4>[12:53]</h4>

It is important to note that the Input names on the Custom node are case sensitive. For example, “uv” can <b>NOT</b> be changed to “UV” without the code needing to be changed.
<br><br>
Be sure to replace the default code in the Custom node with the code provided.
<br><br>
</p>

<p id="[14:08]"><h4>[14:08]</h4>

Open the VizParentPawn Blueprint.
<br><br>
<em>*If you do not want movement functionality for your player, all you need for the following section is to follow the setup instructions for Event BeginPlay, and connect a GetActorTransform node to the SpawnActor node’s Spawn Transform pin as seen starting at [16:40]. </em>
<br><br>
</p>

<p id="[18:29]"><h4>[18:29]</h4>

<em>*If you do not want vignetting during player movement, skip ahead to [30:40].</em>
<br><br>
</p>

<p id="[24:37]"><h4>[24:37]</h4>

You do not need to enter these values yet. At [29:57] a compiler error will require that the nodes be refreshed, and the values will be reset to their defaults. Hold off on entering those values for now.
<br><br>
</p>

<p id="[29:57]"><h4>[29:57]</h4>

This compiler error is just Unreal not understanding that the old input names in the nodes have been changed. Unfortunately, this does mean that the values will need to be input again after the nodes are refreshed if you entered them earlier.
<br><br>
</p>

<p id="[30:40]"><h4>[30:40]</h4>

Back in the VizParentPawn’s EventGraph, set up the Possessed Event.
<br><br>
</p>

<p id="[31:13]"><h4>[31:13]</h4>

Now, open the VizCameraReceiverPawn Blueprint, and follow the video to set up the camera for correct use in the Visualization Studio.
<br><br>
</p>

<p id="[31:55]"><h4>[31:55]</h4>

Lastly, open the Project Settings, and change the Default GameMode and Game Instance classes to VizGameMode and VizStudioGameInstance, respectively.
<br><br>
</p>

</div>