---
layout: home
title: MaiBot Documentation Center
hero:
  name: MaiBot
  text: Multi-model, Human-like, Extensible Intelligent Agent
  tagline: Multiple model collaboration, biomimetic thinking architecture, modular design and internal extensibility bring human-like interaction experience
  image:
    src: /images/mai.png
    alt: MaiBot
  actions:
    - theme: brand
      text: Features
      link: /features
    - theme: brand
      text: User Guide
      link: /manual/
    - theme: alt
      text: Development Docs
      link: /develop/
features:
  - icon: 🧠
    title: Multiple LLMs
    details: Based on multiple LLMs working together, providing natural language understanding and generation capabilities
  - icon: 💾
    title: Memory Capabilities
    details: Can remember events from conversations and how humans speak
  - icon: ❤️
    title: Biomimetic Thinking
    details: Modular design referencing cognitive science theories, with extensibility
  - icon: 🔧
    title: Flexible Configuration
    details: Supports multiple API services, easy to achieve personalized settings
  - icon: 🚢
    title: Multiple Deployment Options
    details: Supports launcher, Docker, Linux, Windows, and various deployment methods
  - icon: 🔄
    title: Continuous Updates
    details: Regular updates and improvements, continuously enhancing functionality and performance
---

## Get More Support

- Visit [GitHub Repository](https://github.com/MaiM-with-u/MaiBot) to submit issues or contribute code
- Join user discussion groups for help

<style scoped>
#star-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

.VPHome {
  position: relative;
  z-index: 1;
}
</style>

<canvas id="star-canvas"></canvas>

<script setup>
import { onMounted, onUnmounted, nextTick } from 'vue'

// Normal cover image list
const normalImages = [
  '/title_img/mai.png',
  '/title_img/mai2.png',
  '/title_img/emoji1.png',
  '/title_img/emoji2.png',
  '/title_img/emoji3.png',
]

// Hidden image (appears with 1/10 probability of other images)
const hiddenImage = '/title_img/dis.png'

let animationFrameId = null
let particles = []

onMounted(async () => {
  await nextTick()
  
  // Weighted random selection: dis.png probability is 1/5 of other images
  // Create a weighted array: other images appear 5 times each, hidden image appears 1 time
  const weightedImages = [
    ...normalImages.map(img => Array(5).fill(img)).flat(), // Each normal image appears 5 times
    hiddenImage // Hidden image appears 1 time
  ]
  
  // Randomly select an image
  const randomImage = weightedImages[Math.floor(Math.random() * weightedImages.length)]
  
  // Try multiple selectors to find hero image
  const selectors = [
    '.VPHomeHero .VPImage img',
    '.VPHomeHero img',
    'main .VPImage img',
    '[alt="MaiBot"]'
  ]
  
  let heroImage = null
  for (const selector of selectors) {
    heroImage = document.querySelector(selector)
    if (heroImage) break
  }
  
  // Function to set image
  const setImage = (imgElement, imageSrc) => {
    imgElement.src = imageSrc
    imgElement.alt = 'MaiBot'
    // If it's emoji4.png, scale to 1.5x
    if (imageSrc.includes('emoji4.png')) {
      imgElement.style.transform = 'scale(1.5)'
      imgElement.style.transformOrigin = 'center'
    } else {
      // Reset scaling for other images
      imgElement.style.transform = ''
      imgElement.style.transformOrigin = ''
    }
  }
  
  // If image element is found, replace it
  if (heroImage) {
    setImage(heroImage, randomImage)
  } else {
    // If not found, try again after delay (waiting for VitePress rendering)
    setTimeout(() => {
      for (const selector of selectors) {
        heroImage = document.querySelector(selector)
        if (heroImage) {
          setImage(heroImage, randomImage)
          break
        }
      }
    }, 100)
  }
  
  // Initialize star effect
  initStarEffect()
})

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId)
  }
})

function initStarEffect() {
  const canvas = document.getElementById('star-canvas')
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  let width = canvas.width = window.innerWidth
  let height = canvas.height = window.innerHeight
  
  const config = {
    spawnRate: 12,
    startSpeed: 0.6,
    attraction: 0.015,
    mouseForce: 0.05,
    maxMouseForce: 1.5,
    maxStarSpeed: 3.0,
    friction: 0.98,
    minDriftSpeed: 0.3,
    starBaseSize: 4,
    circleRadius: 600
  }
  
  const mouse = {
    x: undefined,
    y: undefined,
    vx: 0,
    vy: 0,
    lastX: 0,
    lastY: 0,
    isMoving: false,
    timer: null
  }
  
  const resize = () => {
    width = canvas.width = window.innerWidth
    height = canvas.height = window.innerHeight
  }
  
  window.addEventListener('resize', resize)
  
  window.addEventListener('mousemove', (e) => {
    mouse.x = e.x
    mouse.y = e.y
    mouse.vx = e.x - mouse.lastX
    mouse.vy = e.y - mouse.lastY
    mouse.lastX = e.x
    mouse.lastY = e.y
    mouse.isMoving = true
    
    clearTimeout(mouse.timer)
    mouse.timer = setTimeout(() => {
      mouse.vx = 0
      mouse.vy = 0
      mouse.isMoving = false
      mouse.x = undefined
      mouse.y = undefined
    }, 100)
  })
  
  class Star {
    constructor(centerX, centerY) {
      const angle = Math.random() * Math.PI * 2
      const radius = Math.random() * config.circleRadius * 0.3 + config.circleRadius * 0.1
      this.x = centerX + Math.cos(angle) * radius
      this.y = centerY + Math.sin(angle) * radius
      
      const driftAngle = angle + (Math.random() - 0.5) * 0.5
      const speed = config.startSpeed + Math.random() * 0.3
      
      this.vx = Math.cos(driftAngle) * speed
      this.vy = Math.sin(driftAngle) * speed
      
      this.size = Math.random() * 5 + config.starBaseSize
      this.life = 1
      this.decay = Math.random() * 0.001 + 0.0015
      this.hue = Math.random() * 60 + 180
    }
    
    draw(ctx) {
      ctx.save()
      ctx.translate(this.x, this.y)
      ctx.rotate(this.life * 0.5)
      
      ctx.beginPath()
      const r = this.size
      ctx.moveTo(0, -r)
      ctx.quadraticCurveTo(0, 0, r, 0)
      ctx.quadraticCurveTo(0, 0, 0, r)
      ctx.quadraticCurveTo(0, 0, -r, 0)
      ctx.quadraticCurveTo(0, 0, 0, -r)
      ctx.closePath()
      
      const gradient = ctx.createRadialGradient(0, 0, 0, 0, 0, r)
      gradient.addColorStop(0, `hsla(${this.hue}, 80%, 80%, ${this.life})`)
      gradient.addColorStop(1, `hsla(${this.hue}, 80%, 50%, ${this.life})`)
      
      ctx.fillStyle = gradient
      ctx.fill()
      ctx.restore()
    }
    
    update() {
      if (mouse.x !== undefined) {
        const dx = mouse.x - this.x
        const dy = mouse.y - this.y
        const distance = Math.sqrt(dx*dx + dy*dy)
        
        if (distance < 300) {
          const forceX = dx / distance
          const forceY = dy / distance
          
          this.vx += forceX * config.attraction
          this.vy += forceY * config.attraction
          
          if (mouse.isMoving) {
            let pushX = mouse.vx * config.mouseForce
            let pushY = mouse.vy * config.mouseForce
            
            const pushStrength = Math.sqrt(pushX * pushX + pushY * pushY)
            if (pushStrength > config.maxMouseForce) {
              const scale = config.maxMouseForce / pushStrength
              pushX *= scale
              pushY *= scale
            }
            
            this.vx += pushX
            this.vy += pushY
          }
        }
      }
      
      this.vx *= config.friction
      this.vy *= config.friction
      
      const currentSpeed = Math.sqrt(this.vx * this.vx + this.vy * this.vy)
      if (currentSpeed > config.maxStarSpeed) {
        const scale = config.maxStarSpeed / currentSpeed
        this.vx *= scale
        this.vy *= scale
      }
      
      if (currentSpeed < config.minDriftSpeed) {
        const heroImage = document.querySelector('.VPHomeHero .VPImage img') || 
                         document.querySelector('.VPHomeHero img')
        if (heroImage) {
          const rect = heroImage.getBoundingClientRect()
          const centerX = rect.left + rect.width / 2
          const centerY = rect.top + rect.height / 2
          const angleToCenter = Math.atan2(this.y - centerY, this.x - centerX)
          this.vx += Math.cos(angleToCenter) * 0.005
          this.vy += Math.sin(angleToCenter) * 0.005
        }
      }
      
      this.x += this.vx
      this.y += this.vy
      this.hue += 0.2
      this.life -= this.decay
    }
  }
  
  let frame = 0
  
  const animate = () => {
    animationFrameId = requestAnimationFrame(animate)
    
    ctx.clearRect(0, 0, width, height)
    
    ctx.globalCompositeOperation = 'lighter'
    
    frame++
    
    // Get icon position
    const heroImage = document.querySelector('.VPHomeHero .VPImage img') || 
                     document.querySelector('.VPHomeHero img')
    
    if (heroImage && frame % config.spawnRate === 0) {
      const rect = heroImage.getBoundingClientRect()
      const centerX = rect.left + rect.width / 2
      const centerY = rect.top + rect.height / 2
      particles.push(new Star(centerX, centerY))
    }
    
    for (let i = particles.length - 1; i >= 0; i--) {
      const p = particles[i]
      p.update()
      p.draw(ctx)
      
      if (p.life <= 0) {
        particles.splice(i, 1)
      }
    }
    
    ctx.globalCompositeOperation = 'source-over'
  }
  
  animate()
}
</script>