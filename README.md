<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>Doncre_330</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Nosifer&display=swap');

        *{margin:0;padding:0;box-sizing:border-box}
        body{
            background:#000;
            font-family:'Nosifer',cursive;
            color:#8B0000;
            display:flex;
            flex-direction:column;
            align-items:center;
            justify-content:center;
            min-height:100vh;
            overflow:hidden;
            cursor:none;
        }

        #mainTitle{
            font-size:4.5rem;
            text-shadow:0 0 10px #ff0000,0 0 20px #ff0000;
            animation:flicker 2s infinite;
            cursor:pointer;
            transition:.3s;
        }
        #mainTitle:hover{transform:scale(1.1)}

        @keyframes flicker{
            0%,19%,21%,23%,25%,54%,56%,100%{opacity:1}
            20%,24%,55%{opacity:.4}
        }

        #circularFrame{
            width:260px;
            height:260px;
            border-radius:50%;
            border:5px solid #8B0000;
            margin:40px 0;
            overflow:hidden;
            box-shadow:0 0 30px #ff0000,inset 0 0 30px #000;
            animation:rotate 10s linear infinite;
            cursor:pointer;
            transition:.5s;
        }
        #circularFrame:hover{transform:scale(1.1)}
        @keyframes rotate{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}

        #circularFrame img{
            width:100%;height:100%;object-fit:cover;
            filter:grayscale(100%) brightness(30%) contrast(200%);
        }

        #horrorText{
            max-width:600px;
            text-align:center;
            font-size:1.4rem;
            line-height:1.6;
            animation:blueWhite 2.5s infinite;
            padding:0 20px;
        }
        @keyframes blueWhite{
            0%{text-shadow:0 0 8px #00f,0 0 16px #00f;color:#00f}
            50%{text-shadow:0 0 8px #fff,0 0 16px #fff;color:#fff}
            100%{text-shadow:0 0 8px #00f,0 0 16px #00f;color:#00f}
        }

        /* tombol Hai */
        #haiBtn{
            margin-top:20px;
            padding:10px 30px;
            font-size:1.2rem;
            font-family:'Nosifer',cursive;
            background:#000;
            color:#fff;
            border:2px solid #8B0000;
            cursor:pointer;
            transition:.3s;
            animation:btnPulse 2s infinite;
        }
        #haiBtn:hover{background:#8B0000;color:#000}
        @keyframes btnPulse{
            0%,100%{box-shadow:0 0 5px #8B0000}
            50%{box-shadow:0 0 20px #8B0000}
        }

        #errorScreen,#hiddenMessage{display:none;position:fixed;inset:0;z-index:9999}
        #errorScreen{background:#000;animation:glitch .5s infinite}
        @keyframes glitch{
            0%{transform:translate(2px,1px) rotate(0deg)}
            25%{transform:translate(-2px,-1px) rotate(-1deg)}
            50%{transform:translate(1px,-2px) rotate(1deg)}
            75%{transform:translate(-1px,2px) rotate(0deg)}
            100%{transform:translate(1px,-1px) rotate(-1deg)}
        }

        #hiddenMessage{
            top:50%;left:50%;
            transform:translate(-50%,-50%);
            font-size:3rem;
            color:#fff;
            text-shadow:0 0 10px #f00,0 0 20px #00f;
            animation:flash .6s infinite;
        }
        @keyframes flash{
            0%,100%{color:#f00;text-shadow:0 0 10px #f00}
            33%{color:#00f;text-shadow:0 0 10px #00f}
            66%{color:#fff;text-shadow:0 0 10px #fff}
        }

        .bloodDrip{
            position:absolute;
            width:2px;height:100px;
            background:linear-gradient(to bottom,transparent,#8B0000);
            animation:dripDown 3s linear infinite;
        }
        @keyframes dripDown{to{transform:translateY(100vh)}}

        .cursor-trail{
            position:fixed;
            width:16px;height:16px;
            background:#8B0000;
            border-radius:50%;
            pointer-events:none;
            animation:fadeOut 1s forwards;
        }
        @keyframes fadeOut{to{opacity:0;transform:scale(0)}}
    </style>
</head>
<body>
    <h1 id="mainTitle">Doncre_330</h1>

    <div id="circularFrame">
        <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Crect width='300' height='300' fill='%23000'/%3E%3Ctext x='50%25' y='50%25' dominant-baseline='middle' text-anchor='middle' fill='%238B0000' font-family='Nosifer' font-size='20'%3EMEREKA MENGAWASIMU%3C/text%3E%3C/svg%3E" alt="Horror Image">
    </div>

    <div id="horrorText">
        Setiap malam, aku mendengar mereka...<br>
        Menangis di balik cermin...<br>
        Menunggumu untuk bergabung...
    </div>

    <!-- Tombol Hai -->
    <button id="haiBtn">Hai</button>

    <!-- Audio memang tetap hidden, tapi bisa dipicu tombol -->
    <audio id="horrorSound" loop>
        <source src="data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhBSuBzvLZiTYIG2m98OScTgwOUarm7blmFgU7k9n1unEiBC13yO/eizEIHWq+8+OWT" type="audio/wav">
    </audio>

    <div id="errorScreen"></div>
    <div id="hiddenMessage">KAMU TIDAK AKAN PERNAH KELUAR</div>

    <script>
        // Fungsi trigger error
        function showError(){
            // mainkan suara
            document.getElementById('horrorSound').play().catch(()=>{});
            const es=document.getElementById('errorScreen');
            es.style.display='block';
            const img=new Image();
            img.src='data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" width="500" height="500"%3E%3Crect width="500" height="500" fill="%23000"/%3E%3Ccircle cx="150" cy="200" r="30" fill="%23fff"/%3E%3Ccircle cx="350" cy="200" r="30" fill="%23fff"/%3E%3Cpath d="M 100 350 Q 250 450 400 350" stroke="%23fff" stroke-width="8" fill="none"/%3E%3C/svg%3E';
            img.style.position='fixed';img.style.top='50%';img.style.left='50%';
            img.style.transform='translate(-50%,-50%)';
            es.appendChild(img);
            setTimeout(()=>{es.style.display='none';es.innerHTML='';},3000);
        }

        // Event tombol Hai
        document.getElementById('haiBtn').addEventListener('click',showError);

        // Event lain
        window.addEventListener('load',()=>{
            // Blood drips
            for(let i=0;i<15;i++){
                const d=document.createElement('div');
                d.className='bloodDrip';
                d.style.left=Math.random()*100+'vw';
                d.style.animationDelay=Math.random()*3+'s';
                document.body.appendChild(d);
            }
        });

        document.getElementById('mainTitle').addEventListener('click',showError);
        document.getElementById('circularFrame').addEventListener('click',()=>{
            const hm=document.getElementById('hiddenMessage');
            hm.style.display='block';
            setTimeout(()=>hm.style.display='none',2000);
        });

        // Cursor trail
        document.addEventListener('mousemove',e=>{
            const t=document.createElement('div');
            t.className='cursor-trail';
            t.style.left=e.clientX-8+'px';
            t.style.top=e.clientY-8+'px';
            document.body.appendChild(t);
            setTimeout(()=>t.remove(),1000);
        });
    </script>
</body>
</html>
