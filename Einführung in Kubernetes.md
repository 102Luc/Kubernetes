[← Zurück zur Übersicht](./README.md)

# Kubernetes – Einführung

## 1. Was ist Kubernetes?

Kubernetes (K8s) ist eine Open-Source-Plattform zur Orchestrierung von Containern.

Du beschreibst den gewünschten Zustand deiner Anwendung (z. B. „3 Instanzen meiner API“), und Kubernetes sorgt automatisch dafür, dass dieser Zustand eingehalten wird.

Beispiel:
- Eine VM fällt aus.
- Kubernetes startet automatisch neue Pods auf anderen Nodes.

Kubernetes verwaltet:
- Container
- Skalierung
- Netzwerk
- Self-Healing
- Rolling Updates

---

## 2. Was benötigt Kubernetes?

### 2.1 Infrastruktur

Minimum für ein echtes Cluster:

- 1 Control Plane Node
- 1–2 Worker Nodes
- Linux (z. B. Ubuntu 22.04)
- 2–4 vCPU pro Node
- 4 GB RAM pro Node (Minimum)
- Statische IPs
- Saubere Netzwerkverbindung zwischen allen Nodes

Optional:
- Load Balancer
- Persistentes Storage
- Monitoring

---

## 2.2 Software-Komponenten

Ein Kubernetes-Cluster besteht aus **Control Plane** und **Worker Nodes**.

"Nodes" = Server (VMs)

Beispiel:
<img width="1000" height="640" alt="image" src="https://github.com/user-attachments/assets/e26af885-bd3b-466d-aac2-102b64679f61" />

---

# Control Plane – Komponenten im Detail

Die Control Plane ist das „Gehirn“ des Clusters.

Folgende Komponenten (Ebenfalls Pods) benötigt ein Control Plane:

---

### kube-apiserver

Der API Server ist die zentrale Schnittstelle des Clusters.

Alles läuft über ihn:
- `kubectl` Befehle
- interne Kommunikation
- Änderungen an Ressourcen

Ablauf:
1. Du sendest `kubectl get pods`
2. Anfrage geht an kube-apiserver
3. API Server liest Daten aus etcd
4. Antwort wird zurückgegeben

Wichtig:
Ohne API Server funktioniert gar nichts.

---

### etcd

etcd ist die verteilte Key-Value-Datenbank des Clusters.

Dort wird gespeichert:
- Cluster-Zustand
- Deployments
- Pods
- ConfigMaps
- Secrets
- Node-Informationen

Wenn etcd kaputt ist → Cluster kaputt.

Deshalb:
- Backups sind kritisch
- In Production meist als Cluster mit mehreren Instanzen

---

### kube-scheduler

Der Scheduler entscheidet:

Auf welchem Worker Node soll ein Pod laufen?

Er berücksichtigt:
- CPU / RAM
- Node-Labels
- Taints & Tolerations
- Affinity-Regeln
- Ressourcenlimits

Wichtig:
Der Scheduler startet keine Pods.
Er entscheidet nur, wo sie laufen sollen.

---

### kube-controller-manager

Der Controller Manager überwacht den gewünschten Zustand.

Beispiel:
Du definierst:
