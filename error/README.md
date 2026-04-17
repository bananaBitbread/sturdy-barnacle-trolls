# hard drive on page
```javascript:
const imgUrl = 'https://cdn.jsdelivr.net/gh/bananabitbread/sturdy-barnacle-trolls@main/error/1.png';
const audioUrl = 'https://cdn.jsdelivr.net/gh/bananabitbread/sturdy-barnacle-trolls@main/error/1.mp3';

const sprites = [];
const MAX_TRAILS = 200;
let trailCount = 0;

// preload audio
const baseAudio = new Audio(audioUrl);
baseAudio.volume = 0.3;

function playSound() {
    try {
        const a = baseAudio.cloneNode();
        a.volume = 0.2;
        a.play().catch(() => {});
    } catch {}
}

function createError(x, y, dx, dy) {
    const img = new Image();
    img.src = imgUrl;

    Object.assign(img.style, {
        position: 'fixed',
        left: x + 'px',
        top: y + 'px',
        zIndex: '999999',
        pointerEvents: 'none',
        userSelect: 'none'
    });

    document.body.appendChild(img);

    sprites.push({
        img: img,
        x: x,
        y: y,
        dx: dx,
        dy: dy
    });

    playSound();
}

function spawnTrail(sprite) {
    if (trailCount > MAX_TRAILS) return;

    const clone = sprite.img.cloneNode();
    clone.style.opacity = '0.7';

    document.body.appendChild(clone);
    trailCount++;

    setTimeout(() => {
        clone.remove();
        trailCount--;
    }, 500);

    playSound();
}

function update() {
    for (let s of sprites) {
        s.x += s.dx;
        s.y += s.dy;

        const w = s.img.width || 64;
        const h = s.img.height || 64;

        if (s.x <= 0 || s.x + w >= window.innerWidth) s.dx *= -1;
        if (s.y <= 0 || s.y + h >= window.innerHeight) s.dy *= -1;

        s.img.style.left = s.x + 'px';
        s.img.style.top = s.y + 'px';

        if (Math.random() < 0.2) {
            spawnTrail(s);
        }
    }

    requestAnimationFrame(update);
}

// start with one
createError(window.innerWidth / 2, window.innerHeight / 2, 5, 5);

// slowly add more
setInterval(() => {
    createError(
        Math.random() * window.innerWidth,
        Math.random() * window.innerHeight,
        (Math.random() - 0.5) * 10,
        (Math.random() - 0.5) * 10
    );
}, 2000);

update();
```
