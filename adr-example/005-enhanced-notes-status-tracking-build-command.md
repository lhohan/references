# ADR-005: Enhance Build Command with Structured Notes Status Tracking

## Status
Accepted

## Date
2025-08-25

## Context

The build Claude Code command's 8-phase structured approach is effective but lacks systematic status synchronization to the notes file during execution. This creates problems when:

- Tasks are interrupted and resumed later
- Users need visibility into current progress
- Multiple phases have complex sub-steps that need tracking

### Current Problem
The build command has scattered notes updates:
- Only 4 mentions of updating notes file across 8 phases
- No systematic status tracking (which phase is active/completed)
- No standardized "current status" visibility
- When resuming tasks, unclear exactly where execution left off

### Key Requirements Identified
1. **Session continuity**: Clear status when resuming interrupted tasks
2. **Real-time progress**: User visibility into current execution state
3. **Systematic tracking**: Consistent status updates throughout process
4. **Simplicity**: Avoid cognitive overhead or tool complexity

## Decision

**Use Enhanced Notes File Structure** rather than TodoWrite integration.

### Implementation Approach
1. **Standardized Status Section**: Require consistent "Current Status" section in notes
2. **Phase Transition Updates**: Mandatory notes update when entering/completing each phase
3. **Step-Level Granularity**: Track progress within complex phases like Implementation
4. **Clear Completion Markers**: Explicit "COMPLETED ✅" and "IN PROGRESS" status indicators

### Structure Template
```markdown
## Current Status
- **Phase**: 3 - Implementation
- **Step**: 4 - Handle edge cases and error resilience
- **Last Updated**: 2025-08-25 14:30

## Phase Status Tracker
- ✅ Phase 1: Task Analysis - COMPLETED
- ✅ Phase 2: Solution Design - COMPLETED
- 🔄 Phase 3: Implementation - IN PROGRESS
- ⏸️ Phase 4: Review - PENDING
- ⏸️ Phase 5: Submit - PENDING
- ⏸️ Phase 6: Iterate - PENDING
- ⏸️ Phase 7: Reflect - PENDING
- ⏸️ Phase 8: Clean Up - PENDING
```

## Consequences

**Positive:**
- Solves session continuity problem without additional tools
- Maintains build command simplicity
- Clear progress visibility for users
- Systematic status discipline without cognitive overhead
- Single source of truth (notes file only)

**Negative:**
- Requires disciplined manual updates (not automatically enforced like TodoWrite)
- More verbose notes file structure
- Relies on human consistency rather than tool enforcement
- LLM 'forget' instructions, so not all updates may happen

## Alternatives Rejected

- **TodoWrite Integration**: Would solve tracking perfectly but adds complexity with dual tracking systems (notes + TodoWrite)
- **Hybrid Approach**: TodoWrite only for complex phases - still introduces tool complexity
- **No Changes**: Keeps current scattered approach that doesn't solve session continuity problem

## Implementation Plan

Enhance `.agents/commands/build.md` with:
1. Required notes file structure template
2. Mandatory status updates at phase transitions
3. Clear instructions for maintaining "Current Status" section
4. Step-level tracking guidance for complex phases
