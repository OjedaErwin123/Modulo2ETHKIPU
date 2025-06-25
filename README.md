# Modulo2ETHKIPU
# 🧾 Smart Contract - Subasta Descentralizada (Módulo 2)

Este repositorio contiene el contrato inteligente `Subasta.sol`, desarrollado para el Trabajo Final del Módulo 2. Implementa una subasta descentralizada sobre la red Ethereum (compatible con Sepolia) con funciones avanzadas de reembolso parcial, extensión de tiempo y manejo seguro de fondos.

---

## 🚀 Características principales

- Gestión de ofertas con depósitos en el contrato.
- Validación de oferta mínima (+5% sobre la actual).
- Extensión automática del tiempo si hay ofertas en los últimos 10 minutos.
- Reembolso parcial de montos anteriores para el mismo usuario.
- Devolución automática a no ganadores.
- Comisión del 2% para el propietario.
- Emisión de eventos para trazabilidad.

---

## 🛠️ Estructura del contrato

### 🧱 Variables principales

| Variable            | Descripción                                                        |
|---------------------|--------------------------------------------------------------------|
| `propietario`       | Dirección del creador del contrato                                 |
| `finSubasta`        | Timestamp en el que finaliza la subasta                            |
| `finalizada`        | Indica si la subasta fue cerrada                                   |
| `comision`          | Porcentaje de comisión a descontar al monto ganador                |
| `ganador`           | Dirección del mayor postor                                         |
| `mejorOferta`       | Monto de la mejor oferta                                           |
| `totalOfertado`     | Mapping: total ofertado por cada dirección                         |
| `historialOfertas`  | Registro de ofertas por usuario (para calcular reembolsos)         |
| `oferentes`         | Lista de direcciones que ofertaron                                 |

---

## ⚙️ Funciones públicas

### `constructor(uint256 _duracionMinutos, uint256 _comision)`
Inicializa el contrato definiendo duración de la subasta y comisión.

### `ofertar() payable`
Permite ofertar una cantidad superior en al menos 5% respecto a la mejor oferta actual. Requiere `msg.value`. Si la oferta es válida dentro de los últimos 10 minutos, extiende el tiempo automáticamente.

### `solicitarReembolsoParcial()`
Permite retirar el excedente de ofertas previas. Solo puede usarse si el usuario ha ofertado más de una vez.

### `finalizarSubasta()`
Finaliza la subasta. Transfiere el valor neto al propietario (descontando comisión), devuelve los fondos a no ganadores y emite evento.

### `mostrarGanador()`
Devuelve el ganador y el monto de su oferta. Solo disponible tras finalizar la subasta.

### `mostrarOfertas()`
Lista las direcciones oferentes junto a sus montos ofertados.

### `tiempoRestante()`
Devuelve los segundos restantes para que finalice la subasta.

---

## 📢 Eventos

| Evento              | Descripción                                                  |
|---------------------|--------------------------------------------------------------|
| `NuevaOferta`       | Emite cuando se registra una nueva oferta válida             |
| `ReembolsoParcial`  | Emite cuando un usuario solicita devolución parcial          |
| `SubastaFinalizada` | Emite cuando la subasta se cierra y se declara un ganador    |

---

## 🔐 Seguridad y buenas prácticas

- Uso de `modifiers` para restringir llamadas fuera de tiempo (`soloAntesDeFin`, etc.).
- Validación robusta en todas las funciones críticas.
- Prevención de errores lógicos con mappings y control de estado.
- Eventos que permiten seguimiento en tiempo real vía blockchain.

---

## 🧪 Pruebas recomendadas en Remix

1. **Deploy en Remix (Remix VM):**
   - `_duracionMinutos`: `3`
   - `_comision`: `2`

2. **Ofertar:**
   - Cambiar de cuenta y ofertar con `1 ether`, luego `1.1 ether`, luego `2 ether`

3. **Solicitar reembolso parcial:**
   - Desde quien hizo 2 ofertas → debe recibir el primer monto

4. **Finalizar la subasta:**
   - Simular paso del tiempo (`evm_increaseTime`)
   - Llamar a `finalizarSubasta()`

5. **Verificar resultados:**
   - `mostrarGanador()`
   - `mostrarOfertas()`
   - `tiempoRestante()`

---

## 📦 Despliegue en Sepolia (instrucciones)

1. Conecta Metamask a la red Sepolia.
2. Usa Remix con “Injected Provider - Metamask”.
3. Despliega el contrato con los argumentos requeridos.
4. Verifica el contrato en [Sepolia Etherscan](https://sepolia.etherscan.io/):
   - Selecciona compilador `0.8.26`
   - Activa optimización
   - Pega el código fuente

---

## 📎 Enlaces requeridos

- **🔗 Contrato verificado en Sepolia:**  
  👉 `https://sepolia.etherscan.io/address/0x...`

- **🗃️ Repositorio GitHub:**  
  👉 `https://github.com/usuario/subasta-modulo2`

---

## 👨‍💻 Autor

Erwin Alejandro Ojeda  
Trabajo Final - Módulo 2  
Subasta: Smart Contract  
