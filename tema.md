<p dir="auto"><strong>Problema 1 - Culoarea de fundal.</strong></p>
<ul>
<li dir="auto">a) Schimbați valoarea inițială a lui <code>_clearColor</code> (de exemplu într-un albastru închis) și rulați.</li>
<li dir="auto">b) Faceți fundalul să își schimbe culoarea în timp, ca în Exemplul 1, dar cu <strong>cele trei componente</strong> (R, G, B) oscilând cu <strong>defazaje</strong> diferite (de exemplu 0, 2,094 și 4,189 radiani, adică 0, 2π/3 și 4π/3). Calculul se face în <code>OnUpdateFrame</code>, folosind timpul scurs; indiciu pentru o componentă:<br>
<code>0.5f + 0.5f * MathF.Sin((float)_elapsed)</code>.</li>
<li dir="auto">c) <em>Întrebare:</em> De ce apelul <code>GL.ClearColor(...)</code> este în <code>OnRenderFrame</code> și nu în <code>OnLoad</code>? Ar mai funcționa animația dacă l-am muta în <code>OnLoad</code>?</li>
</ul>
<p dir="auto"><strong>Problema 2 - Culoarea triunghiului.</strong></p>
<ul>
<li dir="auto">a) Schimbați valoarea lui <code>_tint</code> (de exemplu în <code>new Color4(1.0f, 0.3f, 0.3f, 1.0f)</code>). Pe baza fragment shaderului, explicați de ce triunghiul își schimbă aspectul.</li>
<li dir="auto">b) Tratați tastele <code>1</code>, <code>2</code>, <code>3</code> (<code>Keys.D1</code>, <code>Keys.D2</code>, <code>Keys.D3</code>) astfel încât să seteze nuanța pe roșu / verde / albastru, iar tasta <code>0</code> să revină la alb. Folosiți <code>KeyboardState.IsKeyPressed(...)</code>.</li>
<li dir="auto">c) În <code>Triangle.cs</code>, schimbați culorile din tabloul <code>Vertices</code> (de exemplu: galben, cyan, magenta). <em>Întrebare:</em> Cum se obține culoarea pixelilor din interiorul triunghiului, dacă am dat culori doar vârfurilor?</li>
<li dir="auto">d) Dați <strong>aceeași culoare</strong> tuturor vârfurilor. Ce observați?</li>
<li dir="auto">e) Folosiți <code>SetColor(...)</code> și <code>UpdateVertices()</code> pentru a schimba la rulare culoarea unui singur vârf (de exemplu la apăsarea unei taste).</li>
</ul>
<p dir="auto"><strong>Problema 3 - Triunghi plin și contur.</strong><br>
Modul de rasterizare este o <strong>stare</strong> OpenGL, setată cu <code>GL.PolygonMode</code>. Adăugați în <code>OnRenderFrame</code>, <strong>înainte</strong> de <code>_triangle.Draw()</code>:</p>
<pre class="language-csharp"><code class="language-csharp is-loaded">GL<span class="token punctuation">.</span><span class="token function">PolygonMode</span><span class="token punctuation">(</span>MaterialFace<span class="token punctuation">.</span>FrontAndBack<span class="token punctuation">,</span> PolygonMode<span class="token punctuation">.</span>Line<span class="token punctuation">)</span><span class="token punctuation">;</span>   <span class="token comment">// doar contur</span>
</code><button class="copy-code-button"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-copy"><rect x="8" y="8" width="14" height="14" rx="2" ry="2"></rect><path d="M4 16c-1.1 0-2-.9-2-2V4c0-1.1.9-2 2-2h10c1.1 0 2 .9 2 2"></path></svg></button></pre>
<ul>
<li dir="auto">a) Rulați și observați conturul triunghiului. Schimbați <code>Line</code> în <code>Fill</code> (valoarea implicită) și în <code>Point</code>.</li>
<li dir="auto">b) Introduceți un câmp <code>bool _wireframe</code> și faceți ca tasta <strong>F</strong> să comute între modul contur și modul plin (<code>IsKeyPressed</code>, <strong>nu</strong> <code>IsKeyDown</code>: de ce?).</li>
<li dir="auto">c) <em>Întrebare:</em> Dacă apelați <code>GL.PolygonMode</code> o singură dată în <code>OnLoad</code> și nu mai apelați nimic în <code>OnRenderFrame</code>, ce se întâmplă? Ce principiu OpenGL ilustrează acest lucru?</li>
<li dir="auto">d) <em>Facultativ.</em> Pentru <code>PolygonMode.Point</code> punctele sunt foarte mici; dimensiunea lor se setează cu <code>GL.PointSize(10.0f)</code>. Încercați.</li>
</ul>
<blockquote dir="auto">
<p><strong>Observații.</strong> În profilul Core, <code>GL.PolygonMode</code> se folosește doar cu fețele „față și spate” (<code>FrontAndBack</code>). Numele tipului primului parametru depinde de versiunea OpenTK; dacă Visual Studio semnalează <code>MaterialFace</code>, folosiți <code>TriangleFace.FrontAndBack</code> (IntelliSense vă arată tipul acceptat). Mai mult, grosimea liniilor mai mari de 1 pixel (<code>GL.LineWidth</code>) nu este garantată în profilul Core: nu vă bazați pe ea.</p>
</blockquote>
<p dir="auto"><strong>Problema 4 - Deplasarea triunghiului.</strong><br>
Deplasarea este realizată de uniforma <code>uOffset</code> din vertex shader; în C# o controlează câmpul <code>_offset</code>.</p>
<ul>
<li dir="auto">a) Setați <code>_offset = new Vector3(0.3f, 0.2f, 0.0f);</code> și rulați. Observați că <strong>datele din VBO nu s-au schimbat</strong>; ce s-a schimbat?</li>
<li dir="auto">b) Mișcați triunghiul periodic pe orizontală: <code>_offset.X = 0.5f * MathF.Sin((float)_elapsed);</code> (în <code>OnUpdateFrame</code>).</li>
<li dir="auto">c) Deplasați triunghiul cu <strong>săgețile</strong> tastaturii, cu viteză constantă (de exemplu 1 unitate NDC pe secundă). Folosiți formula <em>viteză × Δt</em>: <code>float dt = (float)args.Time;</code> și, de exemplu, <code>if (KeyboardState.IsKeyDown(Keys.Left)) _offset.X -= viteza * dt;</code>.</li>
<li dir="auto">d) Opriți VSync (secțiunea 5.5) și verificați că viteza de deplasare pe ecran <strong>nu</strong> depinde de numărul de cadre pe secundă. <em>Întrebare:</em> Ce s-ar întâmpla dacă am folosi un pas fix pe cadru (de exemplu <code>_offset.X -= 0.01f;</code>)?</li>
<li dir="auto">e) Deplasați triunghiul până când iese din fereastră. <em>Întrebare:</em> Ce se întâmplă cu părțile care ies din cubul NDC și în ce etapă a pipeline-ului?</li>
</ul>
<p dir="auto"><strong>Problema 5 - Deformarea triunghiului (vârfuri deplasate individual).</strong><br>
Acum modificăm <strong>datele vârfurilor</strong>, nu un parametru al shaderului. Metodele <code>SetPosition(vertex, x, y)</code> și <code>UpdateVertices()</code> din <code>Triangle</code> modifică tabloul din RAM și, respectiv, îl recopiază în VBO.</p>
<ul>
<li dir="auto">a) În <code>OnUpdateFrame</code>, mișcați <strong>doar vârful 2</strong> (cel de sus) pe verticală, de exemplu:<br>
<code>_triangle.SetPosition(2, 0.0f, 0.5f + 0.3f * MathF.Sin((float)_elapsed));</code> urmat de <code>_triangle.UpdateVertices();</code></li>
<li dir="auto">b) <em>Întrebare:</em> Ce se întâmplă dacă uitați <code>UpdateVertices()</code>? De ce?</li>
<li dir="auto">c) Mișcați <strong>cele trei vârfuri în direcții diferite</strong>: vârful 0 stânga-dreapta, vârful 1 sus-jos, vârful 2 sus-jos cu altă frecvență (alegeți voi valorile). Observați cum culorile din interior urmăresc vârfurile.</li>
<li dir="auto">d) <em>Întrebare:</em> Care dintre problemele 4 și 5 transferă mai multe date către GPU la fiecare cadru? Comparați <code>SetVector3</code> (uniformă) cu <code>BufferSubData</code> (18 <code>float</code>). Ce ar însemna asta pentru un model cu un milion de vârfuri?</li>
<li dir="auto">e) Aduceți cele trei vârfuri în <strong>același punct</strong>. Ce se desenează și de ce? (Gândiți-vă la aria triunghiului.)</li>
</ul>
<p dir="auto"><strong>Problema 6 - Dreptunghiul cu EBO</strong> <em>(facultativ)</em>.<br>
Creați o clasă nouă <code>Quad</code> (copiați <code>Triangle.cs</code>) care desenează un dreptunghi din <strong>4 vârfuri</strong> și <strong>6 indici</strong>, conform Fig. 6:</p>
<div class="jelly-table-scroll"><table>
<thead>
<tr>
<th>Vârf</th>
<th>Poziție</th>
<th>Culoare</th>
</tr>
</thead>
<tbody>
<tr>
<td>0</td>
<td>(−0.5, −0.5)</td>
<td>roșu</td>
</tr>
<tr>
<td>1</td>
<td>(0.5, −0.5)</td>
<td>verde</td>
</tr>
<tr>
<td>2</td>
<td>(0.5, 0.5)</td>
<td>albastru</td>
</tr>
<tr>
<td>3</td>
<td>(−0.5, 0.5)</td>
<td>galben</td>
</tr>
</tbody>
</table></div>
<p dir="auto">Indicii sunt <code>{ 0, 1, 2,  2, 3, 0 }</code>, iar <code>DrawElements</code> primește <code>count = 6</code>. Înlocuiți în <code>Game</code> obiectul <code>Triangle</code> cu <code>Quad</code> (sau desenați-le pe amândouă, folosind <code>uOffset</code> pentru a le separa). Activați modul contur de la Problema 3: <em>Întrebare:</em> ce muchie apare în interiorul dreptunghiului și ce reprezintă ea?</p>
<p dir="auto"><strong>Problema 7 - Defecte provocate</strong> <em>(facultativ)</em>. Introduceți, pe rând, câte <strong>un singur</strong> defect în <code>Triangle.cs</code> sau în <code>Game.cs</code>, rulați, <strong>observați și explicați</strong> rezultatul, apoi restaurați codul:</p>
<ul>
<li dir="auto">a) comentați <code>GL.EnableVertexAttribArray(1);</code></li>
<li dir="auto">b) schimbați <code>stride</code> din <code>6 * sizeof(float)</code> în <code>5 * sizeof(float)</code>;</li>
<li dir="auto">c) schimbați <code>_indices.Length</code> din <code>DrawElements</code> în <code>2</code>;</li>
<li dir="auto">d) comentați <code>_shader.Use();</code> din <code>OnRenderFrame</code>;</li>
<li dir="auto">e) adăugați în constructorul clasei <code>Triangle</code> linia <code>GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);</code> <strong>înaintea</strong> lui <code>GL.BindVertexArray(0);</code> (vezi secțiunea 6.3).</li>
</ul>