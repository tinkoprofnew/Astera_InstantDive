# Instant Dive (FPV Drone for Goofy Ahh Anticheats)

> *AsteraClient on top!*

**AsteraClient Discord:** https://discord.gg/Yc8cUtQrFD

<video src="./assets/demo.mp4" controls width="100%"></video>

---

## WTF is that?

I wanted to public my sexy code that I use to hunt newfags and skidder guys on freaky ahh anticheats, because doing stuff like this in this stupid square-head game is fun. It doesn't work on GrimAC servers and forces your game to process up to 20 ticks of gravitational acceleration instantly by simulating Player#tick().

---

## My Shit Code

```kotlin

// you can use this shit for your skiddy skid AutoMace
// Credits: Tinkoprof & AsteraClient

import net.minecraft.client.Minecraft
import net.minecraft.world.entity.Entity
import net.minecraft.world.phys.AABB
import net.minecraft.world.phys.Vec3

class FpvDroneForBlockgame {
    private val mc: Minecraft = Minecraft.getInstance()
    
    var isSimulatingDive = false // i hate john200410
        private set

    fun onTick(target: Entity?, isEnabled: Boolean, diveTicks: Int, hitRange: Double) {
        if (!isEnabled || isSimulatingDive) return
        
        val player = mc.player ?: return
        val currentTarget = target ?: return

        val aimPoint = getBestAimPoint(player.eyePosition, currentTarget.boundingBox)
        val distToHitbox = player.eyePosition.distanceTo(aimPoint)

        if (!player.onGround() &&
            !player.isFallFlying &&
            player.y > currentTarget.y + 1.2 &&
            distToHitbox > hitRange
        ) {
            isSimulatingDive = true
            try {
                val extraTicks = diveTicks.coerceIn(1, 20)
                for (i in 0 until extraTicks) {
                    val p = mc.player ?: break
                    val ptAim = getBestAimPoint(p.eyePosition, currentTarget.boundingBox)
                    
                    if (p.onGround() || p.eyePosition.distanceTo(ptAim) <= hitRange) {
                        break
                    }
                    p.tick() // SICHANLIK YAPMAYA DEVAM EL TURCO
                }
            } finally {
                isSimulatingDive = false
            }
        }
    }

    private fun getBestAimPointAsteraOnTop(eyePos: Vec3, box: AABB): Vec3 {
        return Vec3(
            net.minecraft.util.Mth.clamp(eyePos.x, box.minX + 0.05, box.maxX - 0.05),
            net.minecraft.util.Mth.clamp(eyePos.y, box.minY + 0.1, box.maxY - 0.1),
            net.minecraft.util.Mth.clamp(eyePos.z, box.minZ + 0.05, box.maxZ - 0.05)
        )
    }
}
