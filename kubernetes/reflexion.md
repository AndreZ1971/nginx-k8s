# 🧠 Reflexion – Kubernetes Deployment & Service

---

### 🧪 Warum ist ein Deployment in Kubernetes nicht einfach nur eine etwas andere Version von `docker run --restart=always`?

> *Bericht aus dem geheimen Kubernetes-Labor, Protokoll K8S-2077:*

In den Tiefen des Clusters arbeitet das Deployment nicht wie ein primitiver Startbefehl. Es ist ein intelligenter Manager, ein Zustandserhalter. Während `docker run` einen Container startet und hofft, dass er weiterlebt, **beobachtet** und **steuert** das Deployment kontinuierlich den Ist-Zustand gegen den Soll-Zustand. Es kann versionieren, skalieren, updaten und im Falle eines Desasters auf vorherige Zustände zurückrollen – **automatisch** und **strategisch geplant**. Es denkt. Es heilt. Es **lebt**.

---

### 👨‍🏫 Was tut das Deployment, wenn ein Pod plötzlich verschwindet – und warum ist das nicht einfach nur Magie?

> *Ein Azubi, große Augen. Ich zeige ihm den Cluster.*

„Siehst du, wie dieser Pod weg ist? Jetzt schau genau... da, ein neuer erscheint!“  
„Aber... wer hat ihn gestartet?“  
„Nicht *wer*, sondern *was* – das Deployment! Es nutzt ein ReplicaSet, das ständig die Anzahl gewünschter Pods überwacht. Fehlt einer, wird sofort ein neuer erstellt. Das ist keine Magie – das ist das Prinzip der **Selbstheilung** in Kubernetes. Die gewünschte Anzahl Replikate muss immer erfüllt sein.“

---

### 🎬 Was konntest du beim Rolling Update mit `kubectl get pods -w` beobachten – und wie wird hier sichergestellt, dass es keinen kompletten Ausfall gibt?

> *Titel: „Mission: Update“*

Die erste Welle rollt. Ein Pod wird abgeschaltet. Ein neuer erscheint, Version 2.  
Die nächste Welle kommt. Der Wechsel ist koordiniert. Kein Ausfall. Kein Chaos.  
`maxUnavailable: 1` schützt die Mission – immer mindestens zwei Pods am Leben.  
Die neuen Einheiten ersetzen die alten – **still, sauber, effizient.**

---

### 👽 Wie sorgt der Kubernetes-Service dafür, dass dein Browser-Ping (über NodePort) den richtigen Pod trifft – selbst wenn sich gerade ein Update vollzieht?

> *Alien-Dialog im Orbit über Port 80:*

„Wieso weiß euer Service, wohin er den Traffic senden soll?“  
„Jeder Pod trägt ein Namensschild – ein Label: `app=nginx-example`. Der Service scannt nach passenden Pods mit diesem Label. Selbst wenn alte Pods gehen und neue kommen, bleibt das Label gleich. So findet der Service immer die aktiven Pods – unabhängig von ihrer Version oder IP.“

---

### 📦 In der Deployment-YAML: Welche Angaben betreffen die **Pod-Vorlage**, und welche regeln das Verhalten des Deployments (z. B. Skalierung, Strategie)?

- 🎯 **Selektor & Verhalten (Deployment selbst):**
  - `spec.replicas`
  - `spec.selector`
  - `spec.strategy`

- 📦 **Pod-Vorlage (spec.template):**
  - `spec.template.metadata.labels`
  - `spec.template.spec.containers`
  - `containerPort`, `image`, `name`

- 🔄 = Symbol für Update-Strategie  
- 📦 = Symbol für Pod-Vorlage  
- 🎯 = Symbol für Selektor-Ziel

---

### 🦇 Was passiert mit den Pods, wenn du das Deployment löschst – und warum ist das Verhalten logisch?

> *Batman steht auf einem Dach und spricht zu Robin:*

„Robin, ein Deployment ist mehr als nur ein Befehl – es ist ein Kontrollturm. Wenn wir es löschen, verliert das ReplicaSet seinen Kommandanten. Und ohne Kontrolle – verschwinden auch die Pods. Es ist logisch, mein junger Padawan… äh, Sidekick. Kontrolle ist alles.“