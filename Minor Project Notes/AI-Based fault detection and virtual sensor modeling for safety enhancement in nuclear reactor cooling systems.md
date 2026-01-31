# Abstract:
Reliable sensor measurements are critical for safe operation of nuclear reactor cooling systems, as they directly influence protection logic such as SCRAM and emergency core cooling actuation. However, sensors are prone to drift, bias, noise, and failures, which can cause incorrect state estimation and unsafe control actions. This project proposes an AI‑based fault detection framework using virtual sensors to enhance the reliability of nuclear cooling instrumentation. Due to restricted access to real nuclear data, a multi‑source dataset strategy is adopted: (i) physics‑based digital twin (ODE model), (ii) MATLAB/Simulink cooling‑loop simulation with fault injection, and (iii) the Tennessee Eastman Process (TEP) benchmark for validation. A GRU/LSTM virtual sensor predicts expected sensor values, and residual‑based anomaly detection identifies faults, followed by fault classification. A nuclear safety layer estimates sensor health, severity, and safety impact, while SHAP‑based explainable AI provides interpretable alarms. The system aims to detect faults early, reduce false alarms during transients, and improve nuclear plant safety and reliability.
# Introduction:
Nuclear power plants rely on highly sensitive instrumentation to continuously monitor the reactor coolant system (RCS). Sensors measuring temperature, pressure, and flow directly influence safety mechanisms such as reactor trip (SCRAM), loss‑of‑coolant accident (LOCA) detection, and emergency core cooling system (ECCS) actuation. Any incorrect sensor reading can therefore result in unsafe control actions or unnecessary plant shutdowns.

However, physical sensors are prone to drift, bias, noise, ageing, and sudden failure. These faults often develop slowly and are difficult to detect using traditional rule‑based or threshold methods. In complex nuclear systems with tightly coupled variables, a single faulty sensor can mislead operators and protection logic, increasing safety risk and reducing plant reliability.

To address this challenge, this project proposes an AI‑based fault detection and diagnosis system using virtual sensors for nuclear cooling instrumentation. A virtual sensor estimates the expected value of a physical sensor using correlated measurements from other instruments. Any mismatch between the real and predicted values forms a residual, which is used as a fault signature.
# Literature Survey:
| S. No | Title (Journal, Author, Year)                                                                                                                           | Methodology (Summary)                                              | Identification of Gaps & Limitations                         | Result (Include relevant metrics)                                    |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------------------------- |
| 1     | Kim, J. et al., 2021. *AI-based fault detection in nuclear I&C systems*. Annals of Nuclear Energy                                                       | LSTM models for anomaly detection on reactor sensor time-series    | No explainability, single-plant focus                        | Accuracy ≈ 96%                                                       |
| 2     | Wang, H. et al., 2022. *Virtual sensor modelling for nuclear cooling loops*. Nuclear Engineering and Design                                             | ANN soft sensors to estimate coolant temperature                   | No fault classification                                      | RMSE = 0.38                                                          |
| 3     | Li, Y. et al., 2020. *Residual-based FDD using digital twins*. Reliability Engineering & System Safety                                                  | Physics digital twin + ML residual monitoring                      | Limited fault types                                          | FAR ≈ 3.1%                                                           |
| 4     | Zhao, Q. et al., 2023. *LSTM Autoencoder for sensor anomaly detection*. IEEE Access                                                                     | Reconstruction error–based unsupervised detection                  | No nuclear safety logic                                      | F1 ≈ 0.93                                                            |
| 5     | Sun, M. et al., 2021. *Hybrid digital twin + AI FDD*. Energy AI                                                                                         | Simulink cooling loop + NN correction                              | No explainable module                                        | Detection delay ≈ 4 s                                                |
| 6     | Chen, T. et al., 2022. *Sensor health monitoring in reactors*. Progress in Nuclear Energy                                                               | Statistical residual + ML classifier                               | No severity estimation                                       | Accuracy ≈ 94%                                                       |
| 7     | Patel, R. et al., 2023. *Explainable AI for industrial safety*. Eng. Applications of AI                                                                 | SHAP + ensemble classifier                                         | Not nuclear-tested                                           | Precision ≈ 92%                                                      |
| 8     | Ahmed, S. et al., 2020. *Fault diagnosis using XGBoost*. ISA Transactions                                                                               | Residual features + tree models                                    | No temporal learning                                         | F1 ≈ 0.91                                                            |
| 9     | Zhang, P. et al., 2022. *LSTM for nuclear transient monitoring*. Nuclear Science & Engineering                                                          | Sequence learning for drift/transients                             | No benchmark validation                                      | RMSE ≈ 0.41                                                          |
| 10    | Bonsu, J.O. & Adeoye, M.B., 2025. *AI-Driven Fault Detection and Diagnostics in Nuclear Power Plant Control Systems: A Review*. Sarcouncil J. Eng. & CS | Review of SVM, CNN, LSTM, AE, hybrid digital twins for nuclear FDD | Data scarcity, black-box AI, lack of XAI, cybersecurity risk | Review paper – shows AI improves early fault detection & reliability |
# Research Gaps:
•Lack of real nuclear sensor fault datasets by using multi‑source digital twin and benchmark data.
•Black‑box AI models by integrating explainable AI (SHAP) for transparent fault decisions.
•No safety‑aware interpretation by linking sensor faults to nuclear protection logic (SCRAM, ECCS, LOCA).
•High false alarms during normal reactor transients by learning transient‑aware patterns using deep time‑series models.
•No fault severity estimation by introducing severity and safety‑impact scoring.
•Absence of sensor redundancy by implementing AI‑based virtual sensors.
•No continuous sensor health tracking by generating a real‑time sensor health index.
•Lack of cross‑dataset validation by testing the same AI pipeline on ODE, Simulink, and TEP data.
•No residual‑driven hybrid frameworks by combining virtual sensors with residual‑based anomaly detection.
# Objectives:
•To develop an AI-based virtual sensor framework to estimate safety-critical nuclear cooling parameters (temperature, pressure, flow) using correlated reactor measurements.
•To detect early sensor faults by analyzing residuals between measured and predicted signals using ML and statistical anomaly detection.
•To classify sensor fault patterns such as drift, bias, noise, stuck-at faults, and signal dropout for accurate diagnosis.
•To assess fault severity and safety impact by linking detected faults with nuclear protection functions (SCRAM, LOCA detection, ECCS actuation).
•To minimize false alarms during normal reactor transients by differentiating true plant dynamics from sensor degradation.
•To enhance decision transparency using explainable AI (SHAP) for operator and regulatory confidence.
•To monitor sensor health continuously and generate reliability/health scores to support predictive maintenance.
# Justification for Project SDG:
SDG 7: Affordable and Clean Energy aims to ensure reliable, sustainable, and modern energy for all. Nuclear power is a major source of low‑carbon, base‑load electricity, but its safe and continuous operation depends on highly reliable sensor systems in the reactor cooling loop. Sensor faults such as drift, bias, and noise can lead to incorrect plant state estimation, false reactor trips, or delayed safety actions, which reduce plant availability and increase operational costs.

This project supports SDG 7 by developing an AI‑based, safety‑aware fault detection system that:
- Improves reliability of nuclear cooling instrumentation
- Reduces unplanned shutdowns and false alarms
- Enhances early fault detection and maintenance planning
- Increases operational safety and system efficiency

By strengthening the safety and reliability of nuclear power plants, the proposed system contributes to sustainable, affordable, and clean energy production.

# References:
1. Kim, J., Lee, S., & Park, H., “AI‑based fault detection in nuclear I&C systems,” Annals of Nuclear Energy, vol. 155, 2021.
2. Wang, H., Zhang, Y., & Li, Q., “Virtual sensor modelling for nuclear reactor cooling loops,” Nuclear Engineering and Design, vol. 384, 2022.
3. Li, Y., Chen, Z., & Xu, L., “Residual‑based fault detection using digital twin models,” Reliability Engineering & System Safety, vol. 199, 2020.
4. Zhao, Q., Sun, M., & Liu, X., “LSTM autoencoder for multivariate sensor anomaly detection,” IEEE Access, vol. 11, 2023.
5. Sun, M., Zhao, L., & Wang, J., “Hybrid digital twin and AI framework for fault detection,” Energy AI, vol. 5, 2021.
6. Chen, T., Huang, R., & Gao, Y., “Sensor health monitoring in nuclear power plants,” Progress in Nuclear Energy, vol. 142, 2022.
7. Patel, R., Shah, D., & Mehta, A., “Explainable AI for safety‑critical systems using SHAP,” Engineering Applications of Artificial Intelligence, vol. 113, 2023.
8. Ahmed, S., Rahman, M., & Hasan, T., “Fault diagnosis using XGBoost and residual features,” ISA Transactions, vol. 96, 2020.
9. Zhang, P., Li, X., & Zhou, K., “LSTM‑based monitoring of nuclear transient conditions,” Nuclear Science and Engineering, vol. 196, 2022.
10. Bonsu, J. O., & Adeoye, M. B., “AI‑Driven Fault Detection and Diagnostics in Nuclear Power Plant Control Systems: A Review,” Sarcouncil Journal of Engineering and Computer Sciences, vol. 4, no. 9, pp. 552–559, 2025.
11. Downs, J. J., & Vogel, E. F., “A plant‑wide industrial process control problem,” Computers & Chemical Engineering, vol. 17, no. 3, pp. 245–255, 1993. (TEP dataset)
12. Lundberg, S. M., & Lee, S.‑I., “A unified approach to interpreting model predictions,” Advances in Neural Information Processing Systems (NeurIPS), 2017. (SHAP)





