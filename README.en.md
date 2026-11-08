# AI Fruit Sorter

**An AI sorting line for packing houses: the camera looks at every fruit, the model decides good or bad and fires the ejector. No fatigue, consistent criteria, logged by batch and hour.**

[![Open the tool](https://img.shields.io/badge/Try%20it-jsnn15.github.io%2Fselector--fruta-3ccf7a?style=for-the-badge)](https://jsnn15.github.io/selector-fruta/)
![MobileNetV3](https://img.shields.io/badge/MobileNetV3-~3%20ms%20per%20fruit-111?style=flat-square)
![ONNX Runtime Web](https://img.shields.io/badge/ONNX%20Runtime-in%20the%20browser-555?style=flat-square)
![License](https://img.shields.io/badge/code-Apache--2.0-blue?style=flat-square)

[Español](README.md) · **English**

> The interface is in Spanish.

<a href="https://jsnn15.github.io/selector-fruta/"><img src="docs/demo.gif" alt="Animated line: the camera classifies each fruit and the air ejector removes the bad ones" width="100%"></a>

## The problem

Sorting on packing lines is manual. More bad fruit slips through late in the shift, each sorter applies their own criteria, and nothing records how much bad fruit arrived, from which grower or when.

## How it works

<img src="docs/flow.svg" alt="Camera, MobileNetV3 classifier, threshold, ejector and log" width="100%">

```mermaid
flowchart LR
    F[Fruit on the belt] --> C[Photo]
    C --> M["MobileNetV3<br/>p(bad)"]
    M --> U{"p(bad) > strictness<br/>threshold"}
    U -->|yes| E["Air ejector<br/>synced with the belt"]
    U -->|no| X[Export]
    E --> R[Log by batch and hour]
    X --> R
```

The **strictness threshold** is adjustable live: stricter rejects more borderline fruit, looser lets more through. It is the commercial decision each sorter makes today.

## Results

Classifier trained on 2,240 photos of 8 fruits, fresh and rotten, evaluated on **480 photos it never saw**:

| Metric | Result |
|---|---|
| Good/bad accuracy | 98.8 % |
| Good fruit rejected | 0 of 240 |
| Bad fruit passed | 6 of 240 |
| Time per fruit | ~3 ms |

**Limit:** these are photos of loose fruit on plain backgrounds, not an industrial line. A pilot fine-tunes on the client's line and compares against their sorters before touching the line.

## Files

| File | What it is |
|---|---|
| `index.html` | Animated line with camera, ejector and threshold; try-your-own-photo/camera mode. |
| `sel.js` | Preprocessing and ONNX inference. |
| `model.onnx` | MobileNetV3 classifier. |
| `frutas/`, `frutas.json` | Demo photos. |

## Licenses and credits

- Data: Sultana, N. et al. (2022), *An extensive dataset for successful recognition of fresh and rotten fruits*, Data in Brief 44, 108552, **CC BY 4.0**, via Project AgML.
- MobileNetV3 (torchvision, BSD) running on ONNX Runtime Web (MIT).

---

<sub>Personal project by **Julián Neira Nova**, electrical engineer with 10 years in mining operations · Servicio y Soluciones Tecnológicas DelaCruz SpA. Nothing here comes from clients or employers.
[Portfolio](https://jsnn15.github.io/) · [LinkedIn](https://www.linkedin.com/in/julian-neira-nova/) · [All tools](https://github.com/JSNN15)</sub>
