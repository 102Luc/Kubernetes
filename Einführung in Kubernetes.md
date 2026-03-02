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

<img width="1000" height="640" alt="image" src="https://github.com/user-attachments/assets/263d46f6-4aa1-41f4-8528-a05a8f6e0b59" />



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

Es laufen nur 2 Pods.

→ Controller erkennt Abweichung  
→ Startet automatisch neuen Pod

Er enthält mehrere Controller, z. B.:
- Node Controller
- Replica Controller
- Endpoint Controller
- Namespace Controller

Er sorgt für Self-Healing.

Beispiel:

<img width="1744" height="1042" alt="image" src="https://github.com/user-attachments/assets/1caba13c-943e-4299-85a9-73cd654a00b9" />


---

# Worker Node – Komponenten im Detail

Worker Nodes führen die eigentlichen Workloads aus.

---

### kubelet

Der kubelet ist der Agent auf jedem Worker.

Er:
- Kommuniziert mit dem API Server
- Startet Container
- Überwacht Pods
- Meldet Status zurück

Wenn kubelet nicht läuft:
Node ist faktisch tot.

---

### Container Runtime (z. B. containerd)

Die Container Runtime startet die Container technisch.

Beispiele:
- containerd
- CRI-O

Früher:
- Docker (nicht mehr Standard)

Runtime ist zuständig für:
- Container starten
- Container stoppen
- Images ziehen

---

### kube-proxy

kube-proxy regelt Netzwerk-Routing auf Node-Ebene.

Er sorgt dafür, dass:
- Services erreichbar sind
- Traffic korrekt an Pods weitergeleitet wird
- Load-Balancing zwischen Pods funktioniert

Er arbeitet meist mit:
- iptables
- oder IPVS

---

## 2.3 CNI (Container Network Interface)

CNI ist nicht Teil der Core-Installation, aber zwingend notwendig.

Sie ermöglicht:
- Pod-zu-Pod Kommunikation
- Netzwerk-Routing zwischen Nodes

Beispiele:
- Calico
- Flannel
- Cilium

Ohne CNI:
Pods können nicht miteinander sprechen.

---

## 3. Vorteile von Kubernetes

### Self-Healing
- Container crashen → Neustart
- Node fällt aus → Pods werden neu verteilt

### Skalierung
- Horizontal Scaling möglich
- Automatisches Scaling (HPA)

### Rolling Updates
- Updates ohne Downtime
- Automatisches Rollback

### Deklarativer Ansatz
Du beschreibst Zustand in YAML.
Kubernetes sorgt für Umsetzung.

### Infrastruktur-Abstraktion
Gleiches Deployment:
- On-Prem
- Cloud
- Hybrid

---

## 4. Technische Informationen

### Architektur

Ein Cluster besteht aus:

- Control Plane
- Worker Nodes
- Pods
- Deployments
- Services
- Namespaces
- PersistentVolumes

---

### Wichtige Objekte

#### Pod
Kleinste deploybare Einheit.
Enthält 1+ Container.

#### Deployment
Verwaltet Pods.
Ermöglicht:
- Skalierung
- Updates
- Rollbacks

#### Service
Abstraktion über Pods.
Macht sie erreichbar.

#### Namespace
Logische Trennung im Cluster.

---

### Netzwerkmodell

- Jeder Pod erhält eine eigene IP.
- Pods kommunizieren direkt miteinander.
- Services abstrahieren dynamische Pod-IPs.
- Kein NAT zwischen Pods.

---

### Storage

Persistente Daten werden über:

- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)

bereitgestellt.

Ohne PV → Daten gehen beim Pod-Neustart verloren.

---

### Sicherheit

- RBAC
- Network Policies
- Secrets
- Service Accounts

---

## 5. Zusammenfassung

Kubernetes ist:

- Ein Container-Orchestrator
- API-zentriert
- Deklarativ
- Self-Healing
- Skalierbar
- Cloud-agnostisch

Es ist komplex, aber extrem mächtig, wenn die Kernkomponenten verstanden sind.
