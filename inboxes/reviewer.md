# Audit queue — reviewer inbox

Append-only. One lesson per line.

**Format:** `<repo>|<sha>|<lesson-path>`

The executor role appends each new lesson here after writing it.

The reviewer role reads lines past `state/bookmarks.md::reviewer-last-sha`
and audits each lesson — updating the lesson's frontmatter `verdict:` field.

<!-- Executor will append lesson lines below this line -->
chorus|9d606692df88fc935a1dc274fdfc73f89e4bd53c|lessons/chorus/9d60669.md
chorus|0824cbf969cf1941bf0a96259e4c0edd42ca14b4|lessons/chorus/0824cbf.md
chorus|71f4c9eaf91bc98b6e3c3052e4e68c96c5a92f96|lessons/chorus/71f4c9e.md
chorus|55723b0d5e4a414f4613b8ad7100e3867c74d103|lessons/chorus/55723b0.md
chorus|bc84d3abd7e69cca075d4cbc0283b9c5e381d78b|lessons/chorus/bc84d3a.md
chorus|ce0c7003e62f41c9935c9755b19e48f7c6bbc19b|lessons/chorus/ce0c700.md
chorus|569776398e8c1757af8b4ae04514c287c201b823|lessons/chorus/5697763.md
chorus|03ac9c34b07393aaa27331ee895486114fb2f527|lessons/chorus/03ac9c3.md
chorus|328229b11237dfb2428969544da9496b15c0ffcc|lessons/chorus/328229b.md
chorus|568587a44b9ef6a63c2af8628050d8a69b85d32a|lessons/chorus/568587a.md
chorus|948134bed4597b69163c4e0601f945b2ae5ea976|lessons/chorus/948134b.md
chorus|acedff4581422c77bc3e2d2609befef5c56b467a|lessons/chorus/acedff4.md
chorus|0e8678795a739996c522c1cc4b593cc2699457f1|lessons/chorus/0e86787.md
chorus|a93ef3e3c62f288a019f4ea1bf91c285a8e127a7|lessons/chorus/a93ef3e.md
chorus|d245c64cadf67a5d6670d03941933b78afd2e81e|lessons/chorus/d245c64.md
chorus|4fe2e270aa9e3e0f6c6adf6ad53d4671bbe69beb|lessons/chorus/4fe2e27.md
chorus|c7c4870a3f297b9f100d3d491a3c8b4780c11308|lessons/chorus/c7c4870.md
chorus|32f16abee28ea38ca8fb67cad76cf012447b63f6|lessons/chorus/32f16ab.md
chorus|e9407b1d705f632f29390c26e4e0f58cd1552d7c|lessons/chorus/e9407b1.md
chorus|8034565790f85919d6b50be003735c68db3cb1dc|lessons/chorus/8034565.md
chorus|742c6c7cc0b648b786a35660dae100e161173444|lessons/chorus/742c6c7.md
chorus|801173b0d8ba46c2a4c7edbe761bc730c8d0ee39|lessons/chorus/801173b.md
chorus|07ceb4fd34964f4d2ce2f8afed0a8111b9afa562|lessons/chorus/07ceb4f.md
chorus|9ee02baff6a8cb6c6e11fac88985fc09d24efdd4|lessons/chorus/9ee02ba.md
chorus|ed1f2a0087c8feb37cb4c0670a416ea0c3a8fe32|lessons/chorus/ed1f2a0.md
