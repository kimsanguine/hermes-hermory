---
name: hermes-hermory
description: Save, edit, and audit active-profile Hermes memory.
license: MIT
---

# hermes-hermory

Use this skill when the user explicitly asks to save, change, remove, or audit persistent Hermes memory. A request to audit is read-only. Ordinary project work does not authorize new memory writes.

## When to Use

- Save a durable, user-approved preference or working constraint.
- Replace or remove one specific, user-selected memory entry.
- Audit duplicates, stale instructions, temporary facts, and possible secrets without editing them.

Memory stores durable facts and preferences. A skill stores a reusable procedure. Keep temporary results in project documents rather than turning them into memory.

## Procedure

1. Establish the active Hermes profile from the session's actual profile context. If it cannot be established, ask the user before a write. Do not infer a profile name from unrelated files. This skill operates on the active profile only.
2. Discover the memory tools actually available in the current session and read their advertised schemas. Some installations expose `context_notes`; other versions use different tool names. Do not invent a tool, operation, target field, or memory path. If no memory tool is available, stop with a clear setup blocker rather than writing files as a substitute.
3. Read only the relevant current entry or memory state. Do not print unrelated memory or sensitive values in the answer. An audit reports finding categories and masked locations, not secret contents.
4. Show the proposed addition, exact replacement, or deletion and its intended target. Obtain explicit authorization for that concrete change. If the user has already approved that exact change in this conversation, continue without asking again. A request for a draft is not write approval.
5. Use the existing memory tool with the approved scope. For replace/remove, match the freshly read entry, not an old prompt snapshot. If a match fails, reread once and explain the mismatch; do not keep guessing.
6. Inspect the actual tool result and reread the relevant stored state where supported. Report saved, unchanged, rejected, or unverifiable accurately. Explain when immediate visibility differs from loading in a new session.

## Capacity and Conflicts

- Respect the current tool's reported budget. Never assume a fixed character limit across Hermes versions.
- If full, propose a specific duplicate or obsolete item to remove or shorten. Obtain approval before removing unrelated entries.
- Combine trimming and adding in one atomic batch only when the tool explicitly supports it and both changes were approved. Otherwise explain the separate operations and verify each outcome.
- A quota error in an external memory service does not prove local memory is full. Keep local memory and optional external services separate; this skill does not configure external services.

## Pitfalls

- Do not save tokens, passwords, customer data, private conversations, unapproved personal facts, or temporary teaching inputs as durable memory.
- Never copy all existing memory into a public report or source repository.
- Do not change another profile, memory-provider configuration, account identifiers, or retention policy as part of a local audit.
- Never claim a write succeeded from the proposed text or from an assistant promise alone. Use the tool result and available reread evidence.
- This skill is procedural guidance, not a permission-enforcement mechanism. Respect the host's actual approval controls.

## Five-minute Practice

Use a learner-chosen durable work principle, such as “사용을 막는 오류부터 해결하고 색과 간격은 나중에 다룬다.” Show the proposed addition. After approval, save and reread that specific entry. In a new session, apply it to the two feedback items “등록 버튼이 안 눌려요” and “카드 색이 밋밋해요”, citing only the relevant stored principle. If the user wants to add accessibility as a priority, propose replacing that one entry and verify it after approval. For a read-only audit, distinguish this durable principle from a temporary task. Offer removal if the learner no longer wants to keep the practice principle; do not remove it without authorization.

## Verification

Report the active profile, operation, masked target, observed result, and any unverified part. For an audit, report findings and suggested changes without modifying memory. For a write, show only the approved training entry and enough evidence to distinguish the actual stored result from the intended change.
