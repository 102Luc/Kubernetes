[← Zurück zur Übersicht](./README.md)

# Cheat-Sheet

## Kubernetes – Komponenten Überblick

Kubernetes (K8s) besteht im Kern aus einer **Control Plane** (Steuerung) und mehreren **Worker Nodes** (Ausführung). Die Control Plane ist das Gehirn des Clusters und entscheidet, was laufen soll und wo. Die Worker Nodes führen die eigentlichen Container aus.

### Control Plane

Die Control Plane enthält vier zentrale Komponenten:

**kube-apiserver**  
Der API Server ist die zentrale Schnittstelle des Clusters. Jede Aktion – egal ob von `kubectl`, einem internen Controller oder einer Anwendung – läuft über ihn. Er validiert Anfragen, verarbeitet sie und speichert Änderungen im Cluster-Zustand.

**etcd**  
etcd ist die verteilte Key-Value-Datenbank von Kubernetes. Hier wird der komplette Cluster-Zustand gespeichert: Deployments, Pods, Nodes, ConfigMaps, Secrets usw. Wenn etcd nicht verfügbar ist, funktioniert das Cluster nicht mehr korrekt.

**kube-scheduler**  
Der Scheduler entscheidet, auf welchem Worker Node ein neuer Pod gestartet wird. Er berücksichtigt dabei Ressourcen (CPU/RAM), Labels, Affinity-Regeln und andere Constraints. Er startet keine Pods selbst – er trifft nur die Platzierungsentscheidung.

**kube-controller-manager**  
Der Controller Manager überwacht den gewünschten Zustand des Clusters. Wenn du z. B. 3 Replikas definierst, aber nur 2 laufen, erkennt er die Abweichung und startet automatisch einen weiteren Pod. Er sorgt für Self-Healing und Konsistenz.

## Worker Nodes

Worker Nodes führen die Container-Workloads aus und bestehen aus folgenden Komponenten:

**kubelet**  
Der kubelet ist ein Agent, der auf jedem Worker läuft. Er kommuniziert mit dem API Server, startet Container und überwacht den Status der Pods auf diesem Node.

**Container Runtime (z. B. containerd)**  
Die Runtime startet und stoppt Container technisch und zieht Images. Kubernetes selbst startet keine Container – das übernimmt die Runtime.

**kube-proxy**  
kube-proxy verwaltet Netzwerkregeln auf dem Node und sorgt dafür, dass Services korrekt an Pods weiterleiten. Er ermöglicht internes Load-Balancing zwischen Pod-Replikas.

## CNI (Container Network Interface)

Eine CNI-Implementierung (z. B. Calico oder Flannel) ist notwendig, damit Pods über Nodes hinweg miteinander kommunizieren können. Sie implementiert das Kubernetes-Netzwerkmodell. Ohne CNI funktioniert die Pod-Kommunikation nicht.

---

# Was ist kubectl?

`kubectl` ist das Command-Line-Tool zur Steuerung eines Kubernetes-Clusters.

Es ist kein Teil der Control Plane, sondern ein Client, der mit dem **kube-apiserver** kommuniziert. Jede Aktion, die du mit `kubectl` ausführst, wird als API-Anfrage an den Cluster gesendet.

Beispiele:

```bash
kubectl get pods
kubectl apply -f deployment.yaml
kubectl describe pod nginx-xyz
kubectl logs nginx-xyz
