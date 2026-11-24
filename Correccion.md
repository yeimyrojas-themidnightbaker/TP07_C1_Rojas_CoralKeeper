# Corrección, Trabajo Práctico N°07 (Side Scroller 2D)

- **Alumno/a:** Rojas
- **Entrega:** `TP07_C1_Rojas_CoralKeeper`
- **Materia:** Programación con Motores de Videojuegos I
- **Nota TP07:** 8

## 1. Checklist de requisitos

| #   | Requisito | Estado | Evidencia |
|-----|-----------|--------|-----------|
| R1  | Repositorio nuevo con nombre `TP07_C1_Apellido_Titulo` | ✓ | La carpeta y el remoto `origin` de `.git/config` son `TP07_C1_Rojas_CoralKeeper` |
| R2  | Proyecto de Unity con el mismo nombre | ✓ | `TP07_C1_Rojas_CoralKeeper.sln`. Ojo: en `ProjectSettings/ProjectSettings.asset` el `productName` es `CoralKeeper` |
| R3  | El personaje va a la derecha y a la izquierda | ✓ | `Assets/Scripts/Player/PlayerMovement.cs:36-43` (teclas en `PlayerDataSo.RightKeys` / `LeftKeys`), velocidad aplicada en `PlayerMovement.cs:66` |
| R4  | El personaje puede disparar | ✓ | `Assets/Scripts/Player/PlayerShooter.cs:29-33` y `Shoot()` en `PlayerShooter.cs:49-58`, prefab `Assets/Prefabs/Projectile/BubbleProjectile.prefab` |
| R5  | El personaje puede saltar | ✓ | Impulso vertical en `Assets/Scripts/Player/PlayerMovement.cs:72-79` (`AddForce(Vector2.up * SwimUpForce, ForceMode2D.Impulse)`) con el cooldown `TimeBetweenSwims` |
| R6  | Enemigos u obstáculos que le quitan vida al jugador | ✓ | `Assets/Scripts/Player/PlayerHealth.cs:33-66` (contacto con `EnemyMarker`), espinas del pez león en `Assets/Scripts/Projectile/Projectile.cs:23-27` |
| R7  | Se eliminan con más de 2 disparos, tienen vida y barra visible arriba (Canvas World) | ✓ | `Assets/Scripts/Enemies/EnemyHealth.cs:17-26`, vida 4/5/6 en `Assets/Data/*Data.asset`, `Assets/Scripts/UI/UIEnemyHealthBar.cs:20-23`, Canvas en World Space en los tres prefabs de enemigo |
| R8  | ParticleSystem al destruir al enemigo | ✓ | `EnemyHealth.Die()` en `Assets/Scripts/Enemies/EnemyHealth.cs:31` instancia `DeathVfx`; `Assets/Prefabs/VFX/VFXStarfishDeath.prefab`, `VFXMorayDeath.prefab` y `VFXLionfishDeath.prefab` tienen `ParticleSystem` |
| R9  | Hay Pickables | ✓ | `Assets/Scripts/Pickables/Pickable.cs`, `CoralFragment.cs`, `PlanktonPickup.cs`, `SymbiosisPickup.cs`; prefabs en `Assets/Prefabs/CoralFragment.prefab` y `Assets/Prefabs/Pickables/` |
| R10 | Sonido en todas las acciones | ✓ | Música: `Assets/Scripts/Audio/Controllers/BackgroundMusicPlayer.cs`. Saltar: `PlayerMovement.cs:78` (`SwimClip`). Disparar: `PlayerShooter.cs:55` (`ShootClip`). Coleccionable: `Pickable.cs:16-19` (`pickupClip` asignado en los tres prefabs). Eliminar enemigo: `AudioSource` con clip en los tres VFX de muerte. Perder: `Assets/Scripts/Audio/Controllers/ResultSoundPlayer.cs:12` en `GameOver.unity`. Extras: daño (`PlayerHealth.cs:53`) y click de UI (`BtnClickSound.cs`) |
| R11 | Volumen configurable desde Settings conectado a un AudioMixer | ✓ | `Assets/Scripts/Audio/Volume/SliderVolume.cs:21-24` llama a `AudioDataSo.SetVolumeOf`, que hace `mixer.SetFloat` en `Assets/Scripts/Data/AudioDataSo.cs:65`. Parámetros expuestos `VolumeMaster`, `VolumeBackground`, `VolumeSFX` y `VolumeUI` en `Assets/Audio/AudioMixer.mixer`. `PanelSettings.prefab` está en MainMenu y en Gameplay |
| R12 | Usa ScriptableObject para datos o configuraciones | ✓ | `Assets/Scripts/Data/PlayerDataSo.cs`, `EnemyDataSo.cs`, `ProjectileDataSo.cs`, `LevelDataSo.cs`, `CoralProgressSo.cs`, `GameResultSo.cs`, `AudioDataSo.cs`, con sus assets en `Assets/Data/` |
| R13 | Power ups o modificación de jugabilidad | ✓ | `Assets/Scripts/Player/PlayerSymbiosis.cs:15-37`: `SpeedMultiplier` se usa en `PlayerMovement.cs:66` y `FireRateMultiplier` en `PlayerShooter.cs:32` durante `SymbiosisDuration` |
| R14 | Personaje animado | ✓ | `Assets/Scripts/Player/PlayerAnimation.cs:27-74` con los estados Idle, Swim, Jump, Damage, Death; `Assets/Animation/Player/PlayerAnimatorController.controller` |
| R15 | Escena de menú principal previa y otra de Gameplay | ✓ | `ProjectSettings/EditorBuildSettings.asset`: `MainMenu.unity` (índice 0), `Gameplay.unity`, `GameOver.unity` |

## 2. Cumplimiento de Pautas de Cursada

| ID | Pauta | Estado | Evidencia |
|----|-------|--------|-----------|
| P1 | Unity 6000.3.21f1 | ✓ | `ProjectSettings/ProjectVersion.txt` dice `6000.3.21f1` |
| P2 | Repositorio público | ✓ | El repositorio es público |
| P3 | Nombre del repo | ✓ | `TP07_C1_Rojas_CoralKeeper` respeta `TPxx_Cn_Apellido_Titulo` |
| P4 | Código en inglés | ✓ | En los 49 scripts que leí, las clases, métodos, campos, enums y escenas están en inglés. Los comentarios en español están permitidos |
| P5 | Un repo por TP, sin branches | ✓ | Solo existe `main` |

## 3. Estructura de carpetas

| Carpeta esperada | Encontrada | Observación |
|------------------|------------|-------------|
| `Art/Animations/` | ✗ | Las animaciones están en `Assets/Animation/` |
| `Art/Audio/` | ✗ | El audio está en `Assets/Audio/` |
| `Art/Fonts/` | ✓ (como `Art/Font/`) | |
| `Art/Sprites/` | ✓ | |
| `Data/` | ✓ | 11 `.asset` de ScriptableObjects |
| `Prefabs/` | ✓ | Subcarpetas `Audio`, `Pickables`, `Projectile`, `UI`, `VFX` |
| `Scenes/` | ✓ | |
| `Scripts/` | ✓ | Organizada por dominio (`Player`, `Enemies`, `UI`, `Data`, etc.) |

Archivos mal ubicados:

- `Assets/Animation/` va en `Assets/Art/Animations/`.
- `Assets/Audio/` (clips y `AudioMixer.mixer`) va en `Assets/Art/Audio/`.
- `Assets/Prefabs/CoralFragment.prefab` está en la raíz de `Prefabs/`, y los otros dos pickables están en `Assets/Prefabs/Pickables/`.

Cosas que sobran en el repo: `Assets/Prefabs/TestEnemy.prefab` y `Assets/Data/TestEnemyData.asset` no se usan en ninguna escena; `Assets/Art/Sprites/PlaceHolders/` tiene sprites de placeholder; `Assets/InputSystem_Actions.inputactions` y `Assets/WSATestCertificate.pfx` vienen de la plantilla; `Assets/Scripts/AnimationPrefabDestroy/AnimatorStates.cs` es una clase de hashes del Animator que está en una carpeta cuyo nombre no la describe.

## 4. Qué le falta

- Centralizá los nombres de escena en constantes (`"GameOver"`, `"Gameplay"` y `"MainMenu"` aparecen en cuatro scripts).
- No permitas la pausa cuando la partida ya terminó (`UIPauseMenu.cs:50`).
- Hacé que `PlayerHealth.TakeHit` reciba el daño del proyectil.
- Mové `Assets/Animation/` y `Assets/Audio/` dentro de `Assets/Art/`, y `CoralFragment.prefab` a `Prefabs/Pickables/`.
- Borrá `TestEnemy.prefab`, `TestEnemyData.asset` y los placeholders que no usás.

## 5. Hallazgos de código

### Error (0)

### Bug (0)

### Warning (5)

- `Assets/Scripts/GameManager.cs:78` `SceneManager.LoadScene("GameOver")` con string mágico.
- `Assets/Scripts/UI/UIGameOverMenu.cs:34` `"Gameplay"` y `"MainMenu"` como strings mágicos.
- `Assets/Scripts/UI/UIMainMenu.cs:61` `"Gameplay"` hardcodeado, repetido en otros dos scripts.
- `Assets/Scripts/UI/UIPauseMenu.cs:93` `"MainMenu"` y `"Gameplay"` como strings mágicos.
- `Assets/Scripts/UI/UIPauseMenu.cs:50` Escape abre la pausa aunque `GameManager.EndGame` ya se haya ejecutado, y con `timeScale = 0` el `Invoke(nameof(LoadGameOver))` queda congelado.

### Suggestion (4)

- `Assets/Scripts/UI/UIMainMenu.cs:55` El delay `0.5f` es un número mágico que se repite en `OnBtnExitClicked`.
- `Assets/Scripts/Player/PlayerShooter.cs:41` `AnyKeyHeld` está duplicado en `PlayerMovement.cs`.
- `Assets/Scripts/Projectile/Projectile.cs:25` `PlayerHealth.TakeHit()` ignora `projectileData.Damage`.
- `Assets/Scripts/Audio/Volume/VolumePercentageText.cs:31` El `switch` de `OnDisable` repite el de `OnEnable`.

## 6. Recomendaciones

1. Creá una clase de constantes con los nombres de escena y usala en `GameManager`, `UIMainMenu`, `UIPauseMenu` y `UIGameOverMenu`.
2. En `UIPauseMenu.Update`, no permitas la pausa cuando la partida terminó (podés exponer un `IsGameEnded` en `GameManager`).
3. Pasá el daño como parámetro a `PlayerHealth.TakeHit` para que `ProjectileSpineData.Damage` tenga efecto.
4. Reubicá `Assets/Animation/`, `Assets/Audio/` y `Assets/Prefabs/CoralFragment.prefab` según la estructura esperada.
5. Sacá `AnyKeyHeld` a una utilidad compartida y reemplazá el `0.5f` de `UIMainMenu` por una constante.
