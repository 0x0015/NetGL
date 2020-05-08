

NetGL is an open-source .NET 3D visualization engine for your WPF and Windows Forms applications based on OpenGL.

Here you can find source code and samples for NetGL and general 3D and linear algebra algorithms.


NetGL has its own OpenGL bindings and doesn't use 3rd party like OpenTK or SharpGL, so library is lightweight, easy to extend, completely open-source, all on C#, has no deprecated OpenGL bindings. Library is not just a thin OpenGL bindings wrapper, but full-featured 3D engine with animations support, visual three constructor (debugger) application, mouse-picking functionality, off-screen rendering, visual three serialization (in dev), unmanaged resources GC (details below).
You don't need to have deep understanding of OpenGL infrastructure to display and interact with 3D objects using NetGL. 

To display a simple sphere on scene you just need to write this code:


```

using NetGL.Core.Mathematics;
using NetGL.Core.Shaders;
using NetGL.Core.Types;
using NetGL.SceneGraph.Components;
using NetGL.SceneGraph.Scene;
using System.Collections.Generic;
using NetGL.Core.Meshes;

namespace TestScene
{
    public class TestScene : Component
    {
        private readonly List<MeshRenderable> _renderers = new List<MeshRenderable>();
        private readonly Mesh _mesh = NetGL.Core.Meshes.Sphere.Create(0.1f, 10, 10);

        public TestScene(Node owner) : base(owner) {
            Scene.Clear();
            var cameraSO = new Node(Scene, "main camera"); //scene object that will host Camera and CameraManipulator components
            cameraSO.AddComponent<Camera>(); //adding Camera to scene
            cameraSO.AddComponent<CameraManipulator>(); //adding manipulator so you can control camera with (W, A, S, D and mouse Mid) around the scene.

            var sphereSO = new Node(Scene, "spheres"); //scene object that will host sphere mesh renderer component
            sphereSO.Transform.WorldPosition = new Vector3(0, 0, -1); //position object in world space
            var renderer = sphereSO.AddComponent<MeshRenderable>(); //adding component that can render 3D meshes
            var mesh = Icosahedron.Create(0.05f, 3); //generating Icosahedron mesh (sphere)
            renderer.Mesh = mesh; //assigning mesh to renderer

            var lightSO = new Node(Scene, "light0"); //scene object for light source component hosting
            lightSO.Transform.WorldPosition = new Vector3(0, 0, -0.5f); //setting its position
            lightSO.AddComponent<LightSource>(); //adding lightsource component
        }
        
    }
}

```

Code above will result in:

![1sphere.PNG](https://bitbucket.org/repo/rg4neb/images/3849018410-1sphere.PNG)
