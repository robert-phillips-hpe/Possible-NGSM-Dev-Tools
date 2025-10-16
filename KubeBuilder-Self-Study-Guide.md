# KubeBuilder Self-Study Guide (Revised Edition)

> “If you can’t automate your Kubernetes workload, you don’t fully understand it.”  
> — Anonymous SRE

This guide provides a **structured, self-paced curriculum (≈ 35 hours)** for learning KubeBuilder and mastering Operator development on Kubernetes. The revision adds deeper reasoning, measurable learning outcomes, extra resources, and a capstone project to consolidate knowledge.

---

## Table of Contents

1. Course Philosophy  
2. Course Outline (10 Modules + Capstone)  
3. Prerequisites & Required Skills  
4. Detailed Module Breakdown  
5. Capstone Project  
6. Time-Line Overview  
7. Tips for Success  

---

## 1. Course Philosophy

* **Principle-driven** – Knowing _why_ something works is as important as _how_.  
* **Hands-on first** – Every theoretical concept is followed by a practical exercise.  
* **Progressive complexity** – Each module builds on previous ones, culminating in a production-ready Operator.  
* **Vendor-neutral** – Resources span multiple communities, not only KubeBuilder’s official docs.  

---

## 2. Course Outline

| # | Module | Purpose | Est. Time |
|---|--------|---------|-----------|
| 0 | **Capstone Preview** | Define the end goal; design spec for a sample Operator | 0.5 h |
| 1 | Intro to Operators & KubeBuilder | Mental model, landscape, trade-offs | 2 h |
| 2 | Go Fundamentals for Operators | Essential Go (struct tags, generics, contexts) | 4 h |
| 3 | Kubernetes Refresher | Core objects, controller loop, CRDs | 3 h |
| 4 | Environment Setup | Toolchain, local cluster, scaffolding | 2 h |
| 5 | Designing & Generating APIs | Spec vs. Status, validation, defaults | 3 h |
| 6 | Reconcilers in Depth | Idempotency, event filtering, status conditions | 5 h |
| 7 | Testing & Debugging | envtest, Ginkgo, controller-runtime logs | 4 h |
| 8 | Advanced Topics | Webhooks, CRD upgrades, finalizers | 4 h |
| 9 | Packaging & Distribution | Images, manifests, Helm, OLM/OperatorHub | 3 h |
|10 | Best Practices & Security | RBAC, multi-namespace, performance, CVEs | 2 h |
| C | **Capstone Build & Review** | Implement, document, peer-review your Operator | 4 h |
|   | **Total** | | **35 h** |

---

## 3. Prerequisites & Required Skills

1. **Kubernetes Basics**  
   *Why* – The Operator augments the control-plane; you must know its contract.  
   Resources:  
   • Interactive tutorial – <https://kubernetes.io/docs/tutorials/kubernetes-basics/>  
   • “Kubernetes Hard Way” (conceptual read) – <https://github.com/kelseyhightower/kubernetes-the-hard-way>

2. **Go Programming**  
   *Why* – KubeBuilder codebase and your Operator are written in Go.  
   Resources:  
   • Go by Example – <https://gobyexample.com/>  
   • Effective Go – <https://go.dev/doc/effective_go>

3. **Containers & OCI**  
   *Why* – Operators run as containers.  
   Resources:  
   • Docker Official “Get Started” – <https://docs.docker.com/get-started/>  
   • Podman for Docker Users (alternative) – <https://docs.podman.io/en/latest/markdown/podman-for-docker-users.html>

4. **Linux CLI & YAML**  
   *Why* – kubectl, make, shell scripts, manifests.  
   Resources:  
   • Art of Command Line – <https://github.com/jlevy/the-art-of-command-line>  
   • Learn YAML in Y minutes – <https://learnxinyminutes.com/docs/yaml/>

5. **Git & GitHub/GitLab**  
   *Why* – Version control, PR workflow, CI.  
   Resources:  
   • Pro Git book – <https://git-scm.com/book/en/v2>  
   • GitHub Flow – <https://guides.github.com/introduction/flow/>

---

## 4. Detailed Module Breakdown

### 0. Capstone Preview (0.5 h)

**Objective**  
Draft a one-page design doc for an **“AppConfig” Operator** that will:  
• Create a Deployment + ConfigMap per custom resource  
• Restart Pods on ConfigMap changes  
• Expose status conditions (`Available`, `Degraded`)

_No coding yet_; this frames intent.

---

### 1. Introduction to KubeBuilder & Operators (2 h)

• Operator pattern rationale (Day-1 vs Day-2 ops)  
• Survey of frameworks (Operator-SDK, Metacontroller, Java Operator SDK)  
• KubeBuilder architecture (controller-runtime, controller-tools)

Resources  
• Red Hat: What is a Kubernetes Operator? – <https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-operator>  
• The New Stack article – <https://thenewstack.io/kubernetes-operators-automating-the-container-orchestration-platform/>  
• CNCF Webinar “Choosing an Operator Framework” – <https://www.youtube.com/watch?v=Tf8MdtIgDUs>

Exercise  
Write a 200-word comparison of KubeBuilder vs Operator-SDK.

Check-Yourself  
1. When is an Operator over-engineering?  
2. How does controller-runtime differ from client-go?

---

### 2. Go Fundamentals for Operators (4 h)

Learning Outcomes  
• Use struct tags for CRD generation  
• Handle contexts & cancellation  
• Manage Go modules, versioning with `go.work`

Resources  
• Tour of Go – <https://tour.golang.org/>  
• Context primer by Dave Cheney – <https://dave.cheney.net/2019/01/08/contexts-are-for-cancelation>  
• Concurrency in Go (book excerpt) – <https://www.oreilly.com/library/view/concurrency-in-go/9781491941294/>

Exercise  
Refactor a provided spaghetti Go file into idiomatic, testable code using interfaces.

Quiz  
1. What happens if you neglect `ctx.Done()` in long-running reconcilers?  
2. Explain the difference between buffered and unbuffered channels.

---

### 3. Kubernetes Concepts Refresher (3 h)

Focus  
• Desired vs Observed state  
• Informers & listers  
• API Group/Version/Kind

Resources  
• Harness blog on controllers – <https://www.harness.io/blog/kubernetes-controllers-explained/>  
• Mirantis CRD intro – <https://www.mirantis.com/blog/introduction-to-kubernetes-custom-resources/>

Exercise  
Create a CRD manually (YAML) and CRUD it via `kubectl` without any controller.

Checkpoint  
What fields make a CRD _versionable_?

---

### 4. Environment Setup (2 h)

Steps  
1. Install Go >= 1.20  
2. `brew install kubebuilder` _or_ manual install  
3. Kind cluster with Ingress addon  
4. `make install && make run` on scaffolded project

Resources  
• Kind Quick-start – <https://kind.sigs.k8s.io/docs/user/quick-start/>  
• JetBrains GoLand free trial (IDE optional) – <https://www.jetbrains.com/go/>

Exercise  
Automate the entire setup with a Makefile target `make dev-cluster`.

---

### 5. Designing & Generating APIs (3 h)

Concepts  
• Spec vs Status separation (command/query)  
• Validation via Kubebuilder markers (`+kubebuilder:validation:Minimum=1`)  
• Defaulting via webhooks vs open-API defaults  
• Generation workflow (`make generate`, `make manifests`)

Extra Resource  
• BanzaiCloud blog on CRD validation – <https://banzaicloud.com/blog/k8s-crd-validation/>

Exercise  
Add fields `Replicas` (min 1, max 10) and `ConfigTemplate` (string) to `AppConfig` API. Regenerate CRD and install.

Reflection  
Why can’t we update `status` inside the `spec` struct?

---

### 6. Building Reconcilers (5 h)

Key Ideas  
• Controller lifecycle (SetupWithManager, Start)  
• Event filters (Predicates) to reduce reconcile churn  
• Idempotency & edge cases  
• Managing owned resources with `controllerutil.SetControllerReference`  
• Status conditions & `meta.SetStatusCondition`

Resources  
• CNCF blog deep dive – <https://www.cncf.io/blog/2020/07/23/a-deep-dive-into-kubernetes-controllers/>  
• controller-runtime docs – <https://pkg.go.dev/sigs.k8s.io/controller-runtime>

Exercise  
Implement reconcile logic:  
1. Create/Update ConfigMap from `ConfigTemplate`  
2. Create/Update Deployment with hash annotation  
3. Set `Available=True` when Deployment ready

Test Cases (acceptance):  
• Scaling replicas updates Deployment  
• Changing template triggers rollout

Questions  
1. Describe idempotency in the context of `Reconcile`.  
2. How would you avoid an infinite reconcile loop when updating `status`?

---

### 7. Testing & Debugging (4 h)

Topics  
• envtest environment – spinning up a control-plane in process  
• Ginkgo/Gomega for BDD tests  
• Fake client vs envtest trade-offs  
• Debugging with delve and `kubectl logs`

Resources  
• envtest tutorial – <https://book.kubebuilder.io/cronjob-tutorial/test-env.html>  
• “Debugging K8s Operators” – <https://kubernetes.io/blog/2020/07/23/debugging-kubernetes-operators/>

Exercise  
Write an envtest that:  
1. Creates an `AppConfig` CR;  
2. Verifies a Deployment object exists;  
3. Asserts `status.availableReplicas` == spec.replicas.

Thought Question  
Why is `Eventually` required in most controller tests?

---

### 8. Advanced Topics (4 h)

Sub-topics  
• Admission Webhooks (mutating & validating)  
• Finalizers & cleanup ordering  
• CRD versioning (v1alpha1 → v1beta1) with conversion webhooks  
• Performance considerations (QPS, cache size)

Resources  
• Magalix blog on Admission Controllers – <https://www.magalix.com/blog/admission-controllers-in-kubernetes-explained>  
• Fairwinds CRD versioning – <https://www.fairwinds.com/blog/kubernetes-crd-versioning-and-upgrades/>  

Exercise  
Add a mutating webhook that sets `spec.replicas=1` if omitted. Implement a finalizer to delete child Deployments on CR deletion.

Quiz  
1. Difference between “fail-open” and “fail-closed” webhook policies.  
2. Steps to bump CRD version without downtime.

---

### 9. Packaging & Distribution (3 h)

Tasks  
• Multi-stage Docker build with `ko` or `docker-buildx`  
• Helm chart values for namespace/replicas  
• Generate ClusterRole, Role, RoleBinding  
• Publish to OperatorHub (YAML bundle & metadata)

Resources  
• `ko` build tool – <https://ko.build/>  
• OperatorHub docs – <https://operatorhub.io/getting-started>  
• Helm official docs – <https://helm.sh/docs/>

Exercise  
Push your Operator image to a registry (GHCR, Docker Hub) and deploy via Helm on Kind.

Check-Yourself  
Why is `watchNamespaces` environment variable useful for multi-tenant clusters?

---

### 10. Best Practices & Security (2 h)

Discussion  
• Least privilege RBAC & escalating permissions audit  
• Image vulnerability scanning (Trivy, Grype)  
• Observability (Prometheus metrics, structured logs)  
• Handling breaking API changes

Resources  
• CNCF blog on Operator best practices – <https://www.cncf.io/blog/2022/04/19/best-practices-for-writing-kubernetes-operators/>  
• OWASP Kubernetes Top Ten – <https://owasp.org/www-project-kubernetes-top-ten/>

Exercise  
Add Prometheus metrics using `controller-runtime/metrics`. Run Trivy against your image.

Self-Eval  
List three security issues that could arise from a poorly written Operator.

---

## 5. Capstone Project (4 h)

Deliverables  
• Fully functional **AppConfig Operator**  
• Helm chart + README with usage examples  
• Test suite (`make test`) achieving ≥ 80 % coverage  
• Security scan report

Peer Review Checklist  
1. Code readability and docs  
2. Idempotent reconciliation  
3. Proper RBAC and finalizers  
4. Meaningful status conditions  

Optional Stretch  
• HorizontalPodAutoscaler child resource  
• Canary rollout strategy

---

## 6. Time-Line Overview

Week 1 : Modules 0-3 (~9.5 h)
Week 2 : Modules 4-5 (~5 h)
Week 3 : Module 6 (~5 h)
Week 4 : Modules 7-8 (~8 h)
Week 5 : Modules 9-10 + Capstone build (~7.5 h)
Total : ≈ 35 h

Copy Code

---

## 7. Tips for Success

1. **Enable fast feedback** – Use `make run` against Kind; avoid remote clusters early.  
2. **Read generated code** – KubeBuilder scaffolds idiomatic patterns; learn from them.  
3. **Use `kubectl explain`** – Validate CRD fields the API-server sees.  
4. **Join the community** – `#kubebuilder` on Kubernetes Slack, GitHub Discussions.  
5. **Iterate** – Small incremental changes + tests > large rewrites.

---

Happy hacking and may your reconcilers always converge!
