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
