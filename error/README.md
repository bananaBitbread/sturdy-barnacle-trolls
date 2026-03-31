# hard drive on page
`javascript:(function() {
    const imgUrl = 'https://cdn.jsdelivr.net/gh/bananabitbread/sturdy-barnacle-trolls@main/error/1.png';
    const audioUrl = 'https://cdn.jsdelivr.net/gh/bananabitbread/sturdy-barnacle-trolls@main/error/1.mp3';
    
    function createError(x, y, dx, dy) {
        const img = new Image();
        img.src = imgUrl;
        img.style.position = 'fixed';
        img.style.left = x + 'px';
        img.style.top = y + 'px';
        img.style.zIndex = '999999';
        img.style.pointerEvents = 'none';
        document.body.appendChild(img);

        const sound = new Audio(audioUrl);
        sound.play().catch(() => {});

        let posX = x;
        let posY = y;
        let velX = dx;
        let velY = dy;

        function move() {
            posX += velX;
            posY += velY;

            if (posX <= 0 || posX + img.width >= window.innerWidth) velX *= -1;
            if (posY <= 0 || posY + img.height >= window.innerHeight) velY *= -1;

            img.style.left = posX + 'px';
            img.style.top = posY + 'px';

            // Trail effect: Clone on every frame
            const clone = img.cloneNode();
            clone.style.opacity = '0.8';
            document.body.appendChild(clone);
            
            // Play sound for every "clone" created
            new Audio(audioUrl).play().catch(() => {});

            requestAnimationFrame(move);
        }
        requestAnimationFrame(move);
    }

    // Start the chaos from the center
    createError(window.innerWidth/2, window.innerHeight/2, 5, 5);
})();
`
