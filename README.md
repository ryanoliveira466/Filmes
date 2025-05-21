## 🧊 WebGL Shader Visual Demo — Code Breakdown
 
 This demo creates a full-screen WebGL animation using fragment shaders. It visualizes abstract, animated noise patterns in real time. Let's walk through the key components.
 
 ---
 
 ### 🎨 Canvas Setup
 
 ```javascript
 var cn = document.querySelector('canvas'),
     gl = cn.getContext('experimental-webgl');
 ```
 
 * `canvas`: A fullscreen HTML5 drawing surface.
 * `gl`: The WebGL context that enables GPU-accelerated rendering.
 
 ---
 
 ### 📐 Responsive Viewport
 
 ```javascript
 window.onresize = () => {
     cw = cn.width  = window.innerWidth;
     ch = cn.height = window.innerHeight;
     gl.viewport(0, 0, cw, ch);
 };
 onresize();
 ```
 
 * Dynamically resizes the canvas to fit the window.
 * Updates WebGL’s internal resolution to match.
 
 ---
 
 ### 🛠️ Shader Compilation
 
 ```javascript
 function compileShader(gl, source, type) {
     const shader = gl.createShader(type);
     gl.shaderSource(shader, source);
     gl.compileShader(shader);
     ...
 }
 ```
 
 * Converts raw GLSL (shader) code into GPU-executable programs.
 * Handles compile errors gracefully.
 
 ---
 
 ### 🧮 Vertex Shader (Positioning Geometry)
 
 ```glsl
 attribute vec3 avp;
 void main() {
     gl_Position = vec4(avp, 1.0);
 }
 ```
 
 * A minimal shader that outputs vertex positions directly to the screen.
 
 ---
 
 ### 🧱 Geometry Setup (Two Triangles)
 
 ```javascript
 gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([
     -1.0, -1.0,
      1.0, -1.0,
     -1.0,  1.0,
     -1.0,  1.0,
      1.0, -1.0,
      1.0,  1.0
 ]), gl.STATIC_DRAW);
 ```
 
 * Defines two triangles covering the screen.
 * Every pixel fragment will be processed.
 
 ---
 
 ### 🌀 Fragment Shader (Visual Effect)
 
 ```glsl
 precision highp float;
 uniform float uTime;
 uniform vec2 uResolution;
 ...
 void main() {
     vec2 p = gl_FragCoord.xy / uResolution.xy - 0.5;
     ...
     gl_FragColor = vec4(vec3(.4, .3, .7) / rz, 1.0);
 }
 ```
 
 * `uTime`: Tracks animation time.
 * `uResolution`: The screen size.
 * `fbm(p)`: Fractal Brownian Motion (layered noise).
 * `circ(p)`: Adds circular modulation.
 * Final color: Animated purple shades based on noise.
 
 ---
 
 ### 🔁 Animation Loop
 
 ```javascript
 function render() {
     gl.uniform1f(timeLocation, Date.now() - st);
     gl.uniform2fv(resolutionLocation, [cw, ch]);
     gl.drawArrays(gl.TRIANGLES, 0, 6);
     window.requestAnimationFrame(render);
 }
 render();
 ```
 
 * Updates the `uTime` and `uResolution` uniforms each frame.
 * Triggers shader execution.
 * Uses `requestAnimationFrame` for smooth 60fps updates.
 
 ---
 
 ### 🧾 Summary
 
 This shader demo showcases:
 
 * Real-time procedural graphics using only JavaScript and GLSL
 * Fully GPU-powered rendering
 * Fractal noise patterns and circular transformations
 * Minimal CPU load with elegant WebGL rendering
 
 ---
 
 > 📝 **Text Styling Demo**: This block uses standard HTML elements with clear formatting for code, headings, and lists. It's a great way to demonstrate how your site presents structured, readable technical content.
 
 Would you like me to export this as HTML or Markdown for direct use on your site?
