## Appendix. 参考文献・参照情報

本Appendixでは、本文で扱ったAgileの価値観と原則、フレームワーク、品質活動、DevOps、AI支援、発展的な実践を確認するために参照した主要資料を、テーマごとに整理している。

定義や基本的な位置づけについては公式資料や一次情報を優先し、実務上の意味や適用例を理解するために、専門機関や実務経験者による解説も補助的に使用した。また、普及状況やHybrid運用については、業界調査や学術研究を参照している。

掲載資料は関連情報を網羅するものではなく、本文の根拠を確認し、各領域をさらに学ぶための入口として利用することを想定している。調査や事例の結果は、対象や方法、現場の条件が異なるため、すべての組織へそのまま一般化できるものではない。

### A. Core references: Agile values and frameworks

Agileの価値観、原則、Scrum、Kanbanの基本概念を確認するために参照した資料。

- Manifesto for Agile Software Development  
  https://agilemanifesto.org/

- Principles behind the Agile Manifesto  
  https://agilemanifesto.org/principles.html

- The 2020 Scrum Guide  
  https://scrumguides.org/scrum-guide.html

- The Kanban Guide  
  https://kanbanguides.org/

### B. DevOps and technical practices supporting short feedback loops

短いフィードバックループを支えるDevOps、CI/CD、自動テスト、small batch、trunk-based development、monitoring / observabilityなどを確認するために参照した資料。

#### DevOps, Small Batches, and Value Stream

- Microsoft Learn: What is DevOps?  
  https://learn.microsoft.com/en-us/devops/what-is-devops

- DORA: Working in Small Batches  
  https://dora.dev/capabilities/working-in-small-batches/

- DORA: Value Stream Mapping for Software Delivery  
  https://dora.dev/guides/value-stream-management/

#### Continuous Integration, Continuous Delivery, and Trunk-Based Development

- DORA: Continuous Integration  
  https://dora.dev/capabilities/continuous-integration/

- DORA: Continuous Delivery  
  https://dora.dev/capabilities/continuous-delivery/

- DORA: Trunk-Based Development  
  https://dora.dev/capabilities/trunk-based-development/

- Martin Fowler: Continuous Integration  
  https://martinfowler.com/articles/continuousIntegration.html

- Microsoft Learn: What is continuous delivery?  
  https://learn.microsoft.com/en-us/devops/deliver/what-is-continuous-delivery

#### Incremental Change and Release Techniques

- Martin Fowler: Branch By Abstraction  
  https://martinfowler.com/bliki/BranchByAbstraction.html

- Martin Fowler: Parallel Change  
  https://martinfowler.com/bliki/ParallelChange.html

- Microsoft Learn: Progressive experimentation with feature flags  
  https://learn.microsoft.com/en-us/devops/operate/progressive-experimentation-feature-flags

#### Automated Testing and Quality Feedback

- Martin Fowler: The Practical Test Pyramid  
  https://martinfowler.com/articles/practical-test-pyramid.html

#### Monitoring and Observability

- Google SRE Book: Monitoring Distributed Systems  
  https://sre.google/sre-book/monitoring-distributed-systems/

- DORA: Monitoring and Observability  
  https://dora.dev/capabilities/monitoring-and-observability/

- OpenTelemetry: Observability primer  
  https://opentelemetry.io/docs/concepts/observability-primer/

### C. Quality activities in Agile development

アジャイル開発におけるDefinition of Done、受け入れ条件、Shift Left、職種横断の対話、Sprint内のテストや品質活動、mini-waterfallやScrummerfallなどの実務上のアンチパターンを確認するために参照した資料。

#### Testing and Quality Foundations

- ISTQB: Certified Tester Foundation Level Syllabus v4.0.1  
  https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf

#### Definition of Done and Acceptance Criteria

- Scrum.org: What is a Definition of Done?  
  https://www.scrum.org/resources/what-definition-done

- Scrum.org: What Is the Difference Between the Definition of Done and Acceptance Criteria?  
  https://www.scrum.org/resources/blog/what-difference-between-definition-done-and-acceptance-criteria

#### Cross-functional Quality Collaboration

- Cucumber: Who does what?  
  https://cucumber.io/docs/bdd/who-does-what/

#### Quality and Testing within Short Iterations

- Agile Alliance: Slow Down To Go Fast: Lessons Learned Shipping Bing Voice Search on Xbox  
  https://www.agilealliance.org/wp-content/uploads/2016/01/Slow-Down-to-Go-Fast-Lessons-Learned-Shipping-Bing-Voice-Search-on-Xbox.pdf

- Agile Alliance: Help! I am Drowning in 2 Weeks Sprints…How do I determine what NOT to Test!  
  https://agilealliance.org/resources/sessions/help-i-am-drowning-in-2-weeks-sprints-how-do-i-determine-what-not-to-test/

### D. Agile practice, adaptation, and team learning

Scrum、Kanban、Scrumban、Hybrid運用、Jira boardによる作業可視化、チーム改善、現場適応を理解するために参照した資料。公式定義に加え、実務上の運用差、複数手法の組み合わせ、チームや組織の状況に応じた適応を整理するために使う。

#### Kanban and Workflow Visualization

- Kanban University: The Official Guide to The Kanban Method  
  https://kanban.university/kanban-guide/

- Atlassian: A Brief Introduction to Jira Boards  
  https://www.atlassian.com/software/jira/guides/boards/overview

- Atlassian: Kanban vs. Scrum  
  https://www.atlassian.com/agile/kanban/kanban-vs-scrum

#### Scrumban and Hybrid Development

- Agile Alliance: What is Scrumban?  
  https://agilealliance.org/scrumban/

- Atlassian: Scrumban  
  https://www.atlassian.com/agile/project-management/scrumban

- Planview: What is Scrumban?  
  https://www.planview.com/resources/guide/what-is-scrum/lkdc-what-is-scrumban/

- P. Tell *et al.*, "What are Hybrid Development Methods Made Of? An Evidence-Based Characterization," in *Proc. ICSSP*, May 2019, pp. 105–114.  
  https://doi.org/10.1109/ICSSP.2019.00022  
  Open-access preprint: https://arxiv.org/abs/2101.08016

- S. Semenov *et al.*, "Mathematical Model of the Software Development Process with Hybrid Management Elements," *Applied Sciences*, vol. 15, no. 21, p. 11667, Nov. 2025.  
  https://doi.org/10.3390/app152111667

#### Agile Adaptation and Team Learning

- Agile Alliance: Agile 101  
  https://agilealliance.org/agile101/

- Agile Alliance: Experience Reports  
  https://agilealliance.org/resources/experience-reports/

- Diana Larsen and James Shore: The Agile Fluency Model  
  https://martinfowler.com/articles/agileFluency.html

### E. Industry trends and empirical adoption studies

Agile実践の普及状況や、Scrum、Kanban、Scrumban、Hybrid運用などの利用傾向を把握するために参照した資料。

各調査では、対象者、地域、標本抽出方法、設問、回答方式、集計方法が異なる。そのため、数値を単純に比較・平均したり、厳密な世界市場シェアや時系列変化として扱ったりしない。複数の調査でScrumが主要な方法として現れ、Kanbanや複数手法の併用も継続的に確認されるという傾向を把握するための補助資料として扱う。

#### Industry Adoption Surveys

- Digital.ai: 14th Annual State of Agile Report  
  https://www.eg.bucknell.edu/~cs479/common-files/resources/versionone-state-of-agile/14th-annual-state-of-agile-report.pdf

- Digital.ai: 16th State of Agile Report  
  https://scrumgroup.org/wp-content/uploads/AR-SA-2022-16th-Annual-State-Of-Agile-Report.pdf

- Digital.ai: 17th State of Agile Report  
  https://2288549.fs1.hubspotusercontent-na1.net/hubfs/2288549/RE-SA-17th-Annual-State-Of-Agile-Report.pdf

- JetBrains: The State of Developer Ecosystem 2020  
  https://www.jetbrains.com/lp/devecosystem-2020/

- JetBrains: The State of Developer Ecosystem 2024  
  https://www.jetbrains.com/lp/devecosystem-2024/

- Komus et al.: Status Quo (Scaled) Agile 2019/20  
  https://www.ipma.world/assets/Status-Quo-Scaled-Agile-2020_eng_Version_1.0.pdf

#### Academic Empirical Studies

- Licorish *et al.*, 2016. "Adoption and Suitability of Software Development Methods and Practices," in *Proceedings of the 23rd Asia-Pacific Software Engineering Conference (APSEC2016)*. Hamilton, New Zealand, IEEE, pp.369-372.
  https://doi.org/10.1109/APSEC.2016.062  
  Open-access preprint: https://arxiv.org/abs/2103.10653

### F. AI-assisted software development in Agile teams

アジャイル開発へAI支援を取り入れる際の効果と限界、仮説検証による導入、生成量の増加に対してsmall batch、レビュー、テスト、CI、短いフィードバックループを維持する重要性を確認するために参照した資料。

- DORA: State of AI-assisted Software Development 2025  
  https://dora.dev/research/2025/dora-report/

- DORA: Balancing AI tensions: Moving from AI adoption to effective SDLC use  
  https://dora.dev/insights/balancing-ai-tensions/

- Thoughtworks: Using AI for requirements analysis: A case study  
  https://www.thoughtworks.com/insights/blog/generative-ai/using-ai-requirements-analysis-case-study

- Martin Fowler: The Learning Loop and LLMs  
  https://martinfowler.com/articles/llm-learning-loop.html

### G. Further Agile practices, product discovery, and organizational scaling

本資料の中心範囲を越えた発展的な領域として、エンジニアリング実践、Product Discovery、Lean Thinking、人・組織・技術の関係、心理的安全性、複数チーム・組織規模でのAgileを確認するために参照した資料。

#### Engineering Practices and Sustainable Design

- Agile Alliance: What is Extreme Programming (XP)?  
  https://agilealliance.org/glossary/xp/

- Martin Fowler: Refactoring  
  https://refactoring.com/

- Martin Fowler: Is Design Dead?  
  https://martinfowler.com/articles/designDead.html

- Martin Fowler: Technical Debt  
  https://martinfowler.com/bliki/TechnicalDebt.html

#### Product Discovery and Value Exploration

- Silicon Valley Product Group: The Product Operating Model: An Introduction  
  https://www.svpg.com/the-product-operating-model-an-introduction/

- Jeff Patton: User Story Mapping  
  https://jpattonassociates.com/story-mapping/

#### Flow and Lean Thinking

- Lean Enterprise Institute: What is Lean?  
  https://www.lean.org/explore-lean/what-is-lean/

#### Team and Organizational Conditions

- Xebia: A Layman's Introduction To Socio-technical Systems  
  https://xebia.com/blog/a-laymans-introduction-to-socio-technical-systems/

- Atlassian: What does psychological safety mean, anyway?  
  https://www.atlassian.com/blog/teamwork/what-does-psychological-safety-mean-anyway

- Google re:Work: 「効果的なチームとは何か」を知る  
  https://rework.withgoogle.com/intl/jp/guides/understanding-team-effectiveness

#### Multi-team and Large-scale Agile

- Scaled Agile: Scaled Agile Framework  
  https://framework.scaledagile.com/

- Scaled Agile: Lean Portfolio Management Discipline  
  https://framework.scaledagile.com/lean-portfolio-management-discipline

- LeSS: What is LeSS?  
  https://less.works/less/framework

- Scrum.org: Online Nexus Guide  
  https://www.scrum.org/resources/online-nexus-guide
