# RTL8367N — LED Indicators (Standalone Mode)

## Configuración de LEDs para modo Standalone

### Función seleccionada: Link/Act (para todos los puertos)
- **LED encendido fijo** → Link establecido
- **LED parpadeando** → Tráfico activo

---

## Tabla de pines y circuitos

| Puerto | Pin | Nombre | Strapping | Modo LED | Circuito |
|--------|-----|--------|-----------|----------|----------|
| P0 | 75 | P0LED0 | Pull-Up 4.7kΩ | Active Low | LED + 470Ω entre pin y GND |
| P1 | 72 | P1LED0 | Pull-Up 4.7kΩ | Active Low | LED + 470Ω entre pin y GND |
| P2 | 70 | P2LED0 | Pull-Up 4.7kΩ | Active Low | LED + 470Ω entre pin y GND |
| P3 | 68 | P3LED0 | Pull-Down 4.7kΩ | Active High | LED + 470Ω entre pin y +3.3V |
| P4 | 65 | P4LED0 | Pull-Down 4.7kΩ | Active High | LED + 470Ω entre pin y +3.3V |

---

## Circuitos

### Puertos 0, 1, 2 — Active Low (Pull-Up)

```
+3.3V
  │
 [4.7kΩ]  ← Pull-Up (strapping + bias LED)
  │
 Pin ────[470Ω]────[LED▶]──── GND
```

- RTL lleva pin a LOW → LED enciende
- Pin en reposo → HIGH (Pull-Up) → LED apagado
- La resistencia Pull-Up sirve tanto para strapping como para el LED

### Puertos 3, 4 — Active High (Pull-Down)

```
 Pin ────[470Ω]────[LED▶]────+3.3V
  │
 [4.7kΩ]  ← Pull-Down (strapping + bias LED)
  │
 GND
```

- RTL lleva pin a HIGH → LED enciende
- Pin en reposo → LOW (Pull-Down) → LED apagado
- La resistencia Pull-Down sirve tanto para strapping como para el LED

---

## Strapping pins en modo Standalone

| Pin | Nombre | Config | Estado en reset | Modo LED |
|-----|--------|--------|-----------------|----------|
| 65 | EEPROM_MOD | Pull-Down 4.7kΩ | LOW ✅ | Active High |
| 68 | EN_SPIF | Pull-Down 4.7kΩ | LOW ✅ | Active High |
| 69 | DIS_8051 | Pull-Up 4.7kΩ | HIGH ✅ | — |
| 70 | DISAUTOLOAD | Pull-Up 4.7kΩ | HIGH ✅ | Active Low |
| 75 | SMI_SEL | Pull-Up 4.7kΩ | HIGH ✅ | Active Low |

---

## Notas
- Valores según datasheet RTL8367N sección 9.19: **4.7kΩ** pull y **470Ω** serie.
- Las resistencias de strapping sirven simultáneamente como bias del LED. **No se necesitan componentes extra.**
- Pin 72 (Puerto 1) no tiene función strapping: añadir Pull-Up 4.7kΩ dedicado a +3.3V.
- No se usa Gigabit, por lo que LED1 (SPEED) no se conecta en ningún puerto.
- Todos los LEDs muestran función **Link/Act** (fijo = link, parpadeo = tráfico).
