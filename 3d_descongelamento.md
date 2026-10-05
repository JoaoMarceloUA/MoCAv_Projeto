
# Ficha do Modelo Inicial: Modelação do Descongelamento do Gelo em 3D

**Cadeira:** Modelação Computacional Avançada

**Tema:** Problema de Stefan 3D (Mudança de Fase Sólido-Líquido / Fusão do Gelo)

---

### 1. Fenómeno Físico

Transferência de calor tridimensional não-estacionária com **mudança de fase sólido-líquido (fusão/descongelamento do gelo)** num bloco tridimensional, com absorção de calor latente de fusão na interface móvel e recuo gradual do volume de gelo.

---

### 2. Domínio ($\Omega$)

Bloco tridimensional de gelo/água:


$$\Omega = \{ (x,y,z) \in \mathbb{R}^3 \mid 0 \le x \le L_x, \, 0 \le y \le L_y, \, 0 \le z \le L_z \}$$

Onde $(x,y,z)$ representa a geometria do volume (ex.: $L_x = 0.2\,\text{m}$, $L_y = 0.2\,\text{m}$, $L_z = 0.2\,\text{m}$).

---

### 3. Variável Dependente

Campo de temperaturas tridimensional em função do espaço e do tempo:


$$T(x,y,z,t) \quad [^\circ\text{C}]$$

---

### 4. Parâmetros Principais do Sistema

| Parâmetro | Símbolo | Valor / Propriedade | Unidades |
| --- | --- | --- | --- |
| Densidade do Gelo / Água | $\rho$ | $917 - 1000$ | $\text{kg/m}^3$ |
| Condutividade Térmica (Gelo) | $k_{\text{gelo}}$ | $\approx 2.22$ | $\text{W}/(\text{m}\cdot\text{K})$ |
| Condutividade Térmica (Água) | $k_{\text{água}}$ | $\approx 0.58$ | $\text{W}/(\text{m}\cdot\text{K})$ |
| Capacidade Calorífica (Gelo) | $c_{p,\text{gelo}}$ | $\approx 2100$ | $\text{J}/(\text{kg}\cdot\text{K})$ |
| Capacidade Calorífica (Água) | $c_{p,\text{água}}$ | $\approx 4184$ | $\text{J}/(\text{kg}\cdot\text{K})$ |
| Calor Latente de Fusão | $L_f$ | $\approx 334\,000$ | $\text{J/kg}$ |
| Intervalo Numérico de Transição | $\Delta T_{\text{fase}}$ | $[0.0, 0.5]$ | $^\circ\text{C}$ |
| Coef. Convecção na Superfície | $h$ | $\approx 15 - 25$ | $\text{W}/(\text{m}^2\cdot\text{K})$ |

---

### 5. EDP / Modelo Esperado

**Equação Parabólica 3D Não-Linear (Método da Capacidade Calorífica Efetiva):**

$$\rho(T) \, c_{\text{eff}}(T) \, \frac{\partial T}{\partial t} = \frac{\partial}{\partial x} \left( k(T) \, \frac{\partial T}{\partial x} \right) + \frac{\partial}{\partial y} \left( k(T) \, \frac{\partial T}{\partial y} \right) + \frac{\partial}{\partial z} \left( k(T) \, \frac{\partial T}{\partial z} \right)$$

Onde a capacidade calorífica efetiva incorpora o absorvimento de calor latente na temperatura de fusão ($T_f = 0^\circ\text{C}$):

$$c_{\text{eff}}(T) = c_p(T) + L_f \cdot \delta(T - T_f)$$

> **Nota:** A função delta de Dirac $\delta(T - T_f)$ é aproximada numericamente por uma distribuição Gaussiana suave focada no intervalo positivo de mudança de fase ($[0.0^\circ\text{C}, 0.5^\circ\text{C}]$).

---

### 6. Condição Inicial (CI)

Temperatura inicial uniforme de todo o bloco de gelo no instante $t = 0$:

$$T(x,y,z,0) = T_0 \le 0^\circ\text{C} \quad (\text{ex.: } T_0 = -10^\circ\text{C} \text{ ou } -5^\circ\text{C})$$

---

### 7. Condições de Fronteira (CF)

#### A. Superfícies Expostas ao Aquecimento (ex.: Topo $z = L_z$ e Lados $x=L_x, y=L_y$)

* **Opção 1 (Dirichlet):** Temperatura de ar exterior/aquecimento fixa positiva:

$$T_{\text{fronteira}} = T_{\text{amb}} > 0^\circ\text{C} \quad (\text{ex.: } +20^\circ\text{C})$$


* **Opção 2 (Robin):** Convecção de calor com o ambiente quente:

$$-k(T) \, \frac{\partial T}{\partial n} = h \left( T - T_{\text{amb}} \right)$$



#### B. Superfícies de Simetria / Isoladas (ex.: Base $z = 0$ ou planos centrais de simetria $x=0, y=0$)

* **Opção 1 (Neumann):** Fluxo de calor nulo (base ou parede isolada):

$$\frac{\partial T}{\partial n} = 0$$



---

### 8. Significado Físico das CF

* **Superfícies Expostas ($T > 0^\circ\text{C}$):** Modela a transferência contínua de calor do ar quente ou fluido circundante para o bloco de gelo, iniciando o descongelamento das faces exteriores para o interior.
* **Superfícies Isoladas / Planos de Simetria ($\frac{\partial T}{\partial n} = 0$):** Modela a ausência de trocas térmicas com a base de suporte isolada ou permite simular apenas $1/4$ ou $1/8$ do bloco tirando partido da simetria tridimensional.

---

