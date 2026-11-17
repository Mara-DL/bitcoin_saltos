# bitcoin_saltos

Código de la ponencia **«Marco unificado de difusión con saltos para datos financieros de alta frecuencia»**
(CLAIO-ASOCIO 2026): calibración no paramétrica y simulación de BTC/USDT con el esquema de
**Milstein compensado** para una SDE con difusión y saltos.

Autores: Mara Domínguez Limas y Yofre Hernán García Gómez (Universidad Autónoma de Chiapas).

## Qué hace el código

1. **Datos (Fase 0).** Descarga los datos por minutos de BTCUSDT (Binance, enero 2023 – enero 2025), toma el último cierre de cada día y guarda `btc_daily_close.csv`.
2. **Calibración (Fase 1).** Con los rendimientos logarítmicos diarios:
   - varianza continua por **variación bipotencia** (σ̂ diaria);
   - saltos por **umbral** τ = 4·σ̂, de donde salen λ̂ (saltos/año), la media y la desviación de los saltos;
   - descomposición JV/QV y deriva total μ_total (la deriva continua μ_c es sólo diagnóstico).
3. **Simulación (Fase 2).** 1000 trayectorias con Milstein compensado: 2 años de historia (730 pasos, Δt = 1/365) y pronóstico a 30 días.
4. **Resultados (Fases 3–5).** Estadísticos del pronóstico (mediana, P5, P95), figura con zoom y archivo `trayectorias_btc_milstein.npz`.

## Cómo ejecutarlo

```bash
pip install -r requirements.txt
python simulacion_bitcoin.py
```

La primera ejecución descarga los datos (varios cientos de MB en archivos `.zip`, que no se versionan).
Si existe `btc_daily_close.csv`, se usa directamente y no se vuelve a descargar.

Nota: la simulación fija la semilla (`np.random.seed(2026)`), de modo que, con los mismos datos, cada ejecución reproduce las mismas trayectorias y cifras de pronóstico.

## Archivos

| Archivo | Contenido |
|---|---|
| `simulacion_bitcoin.py` | Código completo (descarga, calibración, simulación, figuras) |
| `btc_daily_close.csv` | Cierres diarios BTC/USDT usados en la calibración |
| `requirements.txt` | Dependencias de Python |

## Referencias

- Barndorff-Nielsen, O.E., Shephard, N. (2004). Power and bipower variation with stochastic volatility and jumps. *J. Financ. Econom.* 2(1), 1–37.
- Aït-Sahalia, Y., Jacod, J. (2009). Testing for jumps in a discretely observed process. *Ann. Stat.* 37(1), 184–222.
- Lee, S.S., Mykland, P.A. (2008). Jumps in financial markets: a new nonparametric test and jump dynamics. *Rev. Financ. Stud.* 21(6), 2535–2563.
- Mancini, C. (2009). Non-parametric threshold estimation for models with stochastic diffusion coefficient and jumps. *Scand. J. Stat.* 36(2), 270–296.
- Kloeden, P.E., Platen, E. (1992). *Numerical Solution of Stochastic Differential Equations*. Springer.
- Datos: Binance, https://data.binance.vision
