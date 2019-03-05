# Space Destroyer 2 players

A version of space destroyed to play with another player.

## Code

```blocks
enum SpriteKind {
    Player,
    Projectile,
    Enemy,
    Food
}
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    info.changeScoreBy(-3)
    otherSprite.destroy(effects.ashes, 500)
    scene.cameraShake(4, 500)
})
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprite.destroy()
    otherSprite.destroy(effects.disintegrate, 500)
    info.changeScoreBy(1)
})
controller.player2.onEvent(ControllerEvent.Connected, function () {
    ship2 = sprites.create(img`
        . . . . . . . c d . . . . . . .
        . . . . . . . c d . . . . . . .
        . . . . . . . c d . . . . . . .
        . . . . . . . c b . . . . . . .
        . . . . . . . f f . . . . . . .
        . . . . . . . c 6 . . . . . . .
        . . . . . . . f f . . . . . . .
        . . . . . . . 8 6 . . . . . . .
        . . . . . . 8 8 9 8 . . . . . .
        . . . . . . 8 6 9 8 . . . . . .
        . . . . . c c c 8 8 8 . . . . .
        . . . . 8 8 6 6 6 9 8 8 . . . .
        . . 8 f f f c c e e f f 8 8 . .
        . 8 8 8 8 8 8 6 6 6 6 9 6 8 8 .
        8 8 8 8 8 8 8 8 6 6 6 9 6 6 8 8
        8 8 8 8 8 8 8 8 6 6 6 6 9 6 8 8
    `, SpriteKind.Player)
    ship2.setFlag(SpriteFlag.StayInScreen, true)
    ship2.bottom = 120
    controller.player2.moveSprite(ship2, 100, 100)
})
controller.player2.onEvent(ControllerEvent.Disconnected, function () {
    ship2.destroy()
})
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    projectile = sprites.createProjectileFromSprite(img`
        . . . . . . . .
        . . . . . . . .
        . . . . . . . .
        . . . . . . . .
        . . . 7 7 . . .
        . . . 7 7 . . .
        . . . 7 7 . . .
        . . . 7 7 . . .
    `, ship, 0, -75)
    projectile.startEffect(effects.trail, 500)
})
controller.player2.onButtonEvent(ControllerButton.A, ControllerButtonEvent.Pressed, function () {
    projectile = sprites.createProjectileFromSprite(img`
        . . . . . . . .
        . . . . . . . .
        . . . . . . . .
        . . . . . . . .
        . . . 9 9 . . .
        . . . 9 9 . . .
        . . . 9 9 . . .
        . . . 9 9 . . .
    `, ship2, 0, -75)
    projectile.startEffect(effects.trail, 500)
})
let projectile: Sprite = null
let ship2: Sprite = null
let ship: Sprite = null
let asteroids = [sprites.space.spaceSmallAsteroid1, sprites.space.spaceSmallAsteroid0, sprites.space.spaceAsteroid0, sprites.space.spaceAsteroid1, sprites.space.spaceAsteroid4, sprites.space.spaceAsteroid3]
info.setLife(3)
ship = sprites.create(sprites.space.spaceRedShip, SpriteKind.Player)
ship.setFlag(SpriteFlag.StayInScreen, true)
ship.bottom = 120
controller.moveSprite(ship, 100, 100)
info.startCountdown(30)
game.onUpdateInterval(500, function () {
    projectile = sprites.createProjectileFromSide(asteroids[Math.randomRange(0, asteroids.length - 1)], 0, 75)
    projectile.setKind(SpriteKind.Enemy)
    projectile.x = Math.randomRange(10, 150)
})
```