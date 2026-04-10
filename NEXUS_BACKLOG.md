# 🌌 Nexus Backlog - FaeBits Evolution

Dieses Dokument dient als zentrales Tracking für komplexe System-Upgrades und architektonische Visionen.

## ⏳ Aktive Tasks

### [TASK-001] Implementierung Temporal Decay (Episodic Memory)
**Ziel:** Erinnerungen sollen mit der Zeit verblassen, sofern sie nicht durch Recall gestärkt werden.
- [ ] **Phase 1: Mathematisches Modell**
    - Definition der Zerfallsfunktion (Exponential Decay).
    - Festlegung der $\lambdaParameter für verschiedene Gedächtnis-Tiers.
- [ ] **Phase 2: Datenstruktur-Update**
    - Integration von `created_at` und `last_accessed_at` in die Qdrant-Payloads.
    - Migration bestehender Daten (Default-Timestamp).
- [ ] **Phase 3: Weighted Recall**
    - Anpassung der Abfrage-Logik in `recall.py`.
    - Kombination von semantischem Score und Zeit-Gewichtung.
- [ ] **Phase 4: Memory Strengthening**
    - Implementierung des "Refresh"-Mechanismus bei erfolgreichem Zugriff.
- [ ] **Phase 5: Verifizierung**
    - Erstellung von Test-Cases für zeitabhängige Retrieval-Szenarien.

---
*Zuletzt aktualisiert: 2026-04-10 02:30*
