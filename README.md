# FallingLeafVueJS
VueJS Falling Leaves to mimic an update to a popular java game

```js
<template>
    <div class="falling-particles-container">
        <div
        v-for="(particle, index) in particles"
        :key="index"
        class="particle"
        :style="particle.style"
        ></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            particles: [],
            particleCount: 2, // Number of particles
            styleSheet: null,  // To store reference to our new <style> tag
        };
    },
    methods: {
        generateParticles() {
            // Create a new <style> element if it doesn't exist
            if (!this.styleSheet) {
                const style = document.createElement("style");
                document.head.appendChild(style);
                this.styleSheet = style.sheet;
            }
            
            const particles = [];
            for (let i = 0; i < this.particleCount; i++) {
                const swayAmount = Math.random() * 40 + 10; // Random sway between 10px and 50px
                const animationName = `falling-sway-${i}`;
                this.createRandomKeyframe(animationName, swayAmount);
                
                particles.push(this.createParticle(animationName));
            }
            this.particles = particles;
        },
        createParticle(animationName) {
            const size = Math.random() * 10 + 5; // Particle size
            const left = Math.random() * window.innerWidth; // Random horizontal position
            const animationDuration = Math.random() * 50 + 20; // Random fall duration
            const animationDelay = Math.random() * 2; // Random delay before falling
            // Generate a random number between 0 and 11 for image selection
            const randomLeaf = Math.floor(Math.random() * 12); // 0 to 11
            const imageUrl = `/leaf_${randomLeaf}.png`; // e.g., "leaf_3.png"
            const hueRotation = Math.floor(Math.random() * 360);
            
            return {
                style: {
                    width: `${size}px`,
                    height: `${size}px`,
                    left: `${left}px`,
                    top: "0px", // Ensure particles start at the top
                    position: "absolute",
                    backgroundImage: `url(${imageUrl})`,
                    backgroundSize: "contain",
                    backgroundRepeat: "no-repeat",
                    backgroundPosition: "center",
                    animation: `${animationName} ${animationDuration}s linear infinite`,
                    animationDelay: `${animationDelay}s`,
                    filter: `sepia(100%) hue-rotate(110deg) brightness(80%) saturate(420%)`, // Apply random color shift
                },
            };
        },
        createRandomKeyframe(animationName) {
            if (!this.styleSheet) return;
            
            // Generate random sway positions at different stages
            const sway1 = Math.random() * 150 - 25; // Between -25px and +25px
            const sway2 = Math.random() * 250 - 25;
            const sway3 = Math.random() * 150 - 25;
            
            const keyframes = `
        @keyframes ${animationName} {
          0% { transform: translateY(-100px) translateX(0px); }
          25% { transform: translateY(25vh) translateX(${sway1}px); }
          50% { transform: translateY(50vh) translateX(${sway2}px); }
          75% { transform: translateY(75vh) translateX(${sway3}px); }
          100% { transform: translateY(100vh) translateX(0px); }
        }
      `;
            this.styleSheet.insertRule(keyframes, this.styleSheet.cssRules.length);
        },
        createKeyframe(animationName, swayAmount) {
            if (!this.styleSheet) return; // Ensure we have a stylesheet to modify
            
            const keyframes = `
          @keyframes ${animationName} {
            0% {
              transform: translateY(-100px) translateX(0);
            }
            50% {
              transform: translateY(50vh) translateX(${swayAmount}px);
            }
            100% {
              transform: translateY(100vh) translateX(-${swayAmount}px);
            }
          }
        `;
            this.styleSheet.insertRule(keyframes, this.styleSheet.cssRules.length);
        }
    },
    mounted() {
        this.generateParticles();
    },
};
</script>


<style scoped>
.falling-particles-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
}

.particle {
    position: absolute;
}
</style>
```
