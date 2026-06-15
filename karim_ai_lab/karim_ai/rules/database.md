# Database Rules

## Purpose

This file defines how agents must design, read, write, and modify database-related work inside any project using Karim AI Lab.

Database work is high-risk. A bad schema change can break users, expose private data, corrupt business logic, or make future features harder to build.

These rules apply to SQL, Supabase, Postgres, migrations, RLS, seed data, analytics tables, vector tables, and any database-like storage.

---

## Core Law

Agents must never