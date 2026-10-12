**Problema 0 - Pornirea șablonului.**

c) Întrebare: Este versiunea raportată cel puțin 3.3? Dacă ar fi mai mică, ce s-ar întâmpla la crearea ferestrei?

Da. 

Versiunea 3.2 funcționează, versiuni mai joase produc(3.2 a fost împărțit în profil Core și Compatibility):

**Exception has occurred: CLR/OpenTK.Windowing.GraphicsLibraryFramework.GLFWException <br>
An unhandled exception of type 'OpenTK.Windowing.GraphicsLibraryFramework.GLFWException' occurred in OpenTK.Windowing.<br>
Desktop.dll: 'Context profiles are only defined for OpenGL version 3.2 and above** (this is thrown from OpenTKs default<br> GLFW error handler, if you find this exception inconvenient set your own error callback using GLFWProvider.SetErrorCallback)'<br>
   at OpenTK.Windowing.Desktop.GLFWProvider.DefaultErrorCallback(ErrorCode errorCode, String description)<br>
   at OpenTK.Windowing.GraphicsLibraryFramework.GLFWNative.glfwCreateWindow(Int32 width, Int32 height, Byte* title, Monitor* monitor, Window* share)<br>
   at OpenTK.Windowing.GraphicsLibraryFramework.GLFW.CreateWindow(Int32 width, Int32 height, String title, Monitor* monitor, Window* share)<br>
   at OpenTK.Windowing.Desktop.NativeWindow..ctor(NativeWindowSettings settings)<br>
   at OpenTK.Windowing.Desktop.GameWindow..ctor(GameWindowSettings gameWindowSettings, NativeWindowSettings nativeWindowSettings)<br>
   at Egc.Game..ctor(Int32 width, Int32 height, String title) in C:\Users\Sorin\Downloads\triunghi\Game.cs:line 35<br>
   at Program. $(String[] args) in C:\Users\Sorin\Downloads\triunghi\Program.cs:line 5<br>

**Problema 1 - Culoarea de fundal.**

c) Întrebare: De ce apelul GL.ClearColor(...) este în OnRenderFrame și nu în OnLoad? Ar mai funcționa animația dacă l-am muta în OnLoad?

Pentru a rula la fiecare cadru, nu doar o singură dată. Nu.

**Problema 2 - Culoarea triunghiului.**

c) În Triangle.cs, schimbați culorile din tabloul Vertices (de exemplu: galben, cyan, magenta). Întrebare: Cum se obține culoarea pixelilor din interiorul triunghiului, dacă am dat culori doar vârfurilor?

Culorile sunt interpolate pe baza vârfurilor în procesul de rasterizare.

**Problema 3 - Triunghi plin și contur.**

c) Întrebare: Dacă apelați GL.PolygonMode o singură dată în OnLoad și nu mai apelați nimic în OnRenderFrame, ce se întâmplă? Ce principiu OpenGL ilustrează acest lucru?

Triunghiul rămâne la modul setat în OnLoad. Principiul ilustrat este funcționarea ca state-machine unde starea rămâne până este modificată iarăși.

**Problema 4 - Deplasarea triunghiului.**

d) Opriți VSync (secțiunea 5.5) și verificați că viteza de deplasare pe ecran nu depinde de numărul de cadre pe secundă. Întrebare: Ce s-ar întâmpla dacă am folosi un pas fix pe cadru (de exemplu _offset.X -= 0.01f;)?

Viteza mișcării ar fi bazată pe numărul de cadre pe secundă.

e) Deplasați triunghiul până când iese din fereastră. Întrebare: Ce se întâmplă cu părțile care ies din cubul NDC și în ce etapă a pipeline-ului?

Asamblarea primitivelor, decupare, viewport: Triunghiul este decupat

