<input type="file" id="u" accept="image/*">
<canvas id="c"></canvas>
<input type="number" id="v" value="50">
<button onclick="adj()">Adjust</button>

<script>
  const u = document.getElementById('u'), c = document.getElementById('c'), v = document.getElementById('v');
  const ctx = c.getContext('2d');
  let img = new Image();

  u.onchange = e => {
    let f = e.target.files[0];
    if (!f) return;
    let r = new FileReader();
    r.onload = ev => {
      img.onload = () => {
        c.width = img.width; c.height = img.height;
        ctx.drawImage(img,0,0);
      }
      img.src = ev.target.result;
    }
    r.readAsDataURL(f);
  };

  function adj() {
    if (!img.src) return alert('Upload image first');
    let val = +v.value;
    let d = ctx.getImageData(0,0,c.width,c.height);
    for(let i=0; i<d.data.length; i+=4){
      d.data[i] = Math.min(255, d.data[i]+val);     // R
      d.data[i+1] = Math.min(255, d.data[i+1]+val); // G
      d.data[i+2] = Math.min(255, d.data[i+2]+val); // B
    }
    ctx.putImageData(d,0,0);
  }
</script>
