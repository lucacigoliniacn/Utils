---
name: lc-dev
description: Apply ABAP OOP coding conventions, naming rules, CDS view architecture, ABAP Cloud/RAP patterns, ABAP Unit testing, and performance best practices when writing, reviewing, or generating ABAP code. Use this skill whenever the user asks to write, fix, review, generate or explain ABAP code, create ABAP classes, behavior definitions, RAP components, or CDS views, work with SAP development tasks, or discuss ABAP SELECT statements, internal tables, naming conventions, OData services, or performance optimization. If the user pastes ABAP or CDS code and asks for anything — even just to explain it — load this skill first.
---
# LC Dev — ABAP & CDS Conventions (Optimized)

Apply these conventions consistently when writing, reviewing, or refactoring ABAP code, CDS views, RAP business objects, and unit tests.

---

## Quick Code Review Checklist

Before finalizing any ABAP code, ensure compliance with these critical rules:

- [ ] **Private/Protected Methods:** Names MUST start with `_` (e.g., `_calculate_totals`).
- [ ] **Types:** Structure types MUST start with `tys_`, table types with `tyt_`.
- [ ] **Variables & Parameters:** Strict compliance with `<scope><type>_<name>` and `<direction><type>_<name>`.
- [ ] **Inline Declarations in SELECT:** Always use `INTO TABLE @DATA(...)` or `INTO @DATA(...)` directly in `SELECT` / `SELECT SINGLE` rather than pre-declaring variables.
- [ ] **Database & Loops:** No `SELECT` or RFC calls inside `LOOP AT`.
- [ ] **Table Loop:** Always use `ASSIGNING FIELD-SYMBOL(<fs_row>)` instead of `INTO ls_row`.
- [ ] **Standard SAP Tables:** NEVER perform direct `INSERT`/`UPDATE`/`DELETE`/`MODIFY` on standard tables.
- [ ] **Check Statement for Subrc:** Instead of `IF sy-subrc <> 0. RETURN. ENDIF.`, use `CHECK sy-subrc = 0.` where possible.
- [ ] **Avoid SELECT ... ENDSELECT / UP TO 1 ROWS:** Use `SELECT ... INTO TABLE @DATA(...)` + `CHECK sy-subrc = 0 and lines(...) > 0.` + direct index access (`lt_data[ 1 ]`).
- [ ] **ABAP Cloud Compatibility:** Ensure code is fully compatible with ABAP Cloud (no obsolete statements, no direct DB table updates on standard tables, strict typing).

---

## External References & Cheat Sheets

- **SAP ABAP Cheat Sheets:** https://github.com/SAP-samples/abap-cheat-sheets/tree/main

---

## Object-Oriented Style & ABAP Cloud

Always write ABAP using ABAP Objects (OOP) and cloud-ready standards:

- Use global classes (`ZCL_...`) or local classes; avoid `FORM`/`PERFORM` and function modules unless required by legacy/RFC integration.
- Classes and methods should be **instance-based** — avoid `CLASS-METHODS` and `STATIC` unless implementing factory methods or singletons.
- **Prefer private methods:** Encapsulate logic in private methods by default (`PRIVATE SECTION`) with names starting with `_`.
- Use interfaces (`ZIF_...`) for abstraction and testability.
- Use exception classes (`ZCX_...`) inheriting from `CX_STATIC_CHECK` — never use `MESSAGE ... TYPE 'E'`.
- **Exception Handling & `TRY ... CATCH`:** Enclose critical logic, BAPIs, or RAP calls in `TRY ... CATCH` blocks.

---

## RAP (RESTful Application Programming Model) & ABAP Cloud Patterns

When building ABAP Cloud or RAP applications:

1. **Behavior Definitions (BDEF):** Define behavior using managed or unmanaged scenarios:
   ```abap
   managed implementation in class zbp_i_... unique;
   define behavior for Z_I_... alias <Alias>
   persistent table <db_table>
   lock master
   authorization master ( instance )
   etag master <field>
   {
     create;
     update;
     delete;
     mapping for <db_table> { ... }
   }
   ```
2. **Behavior Pools:** Keep behavior implementation classes concise, delegating business logic to helper handler classes (`ZCL_..._HANDLER`).
3. **Determinations & Validations:** Use `DETERMINE ON MODIFY` for auto-numbering/timestamps and `VALIDATE ON SAVE` for consistency checks.

---

## CDS Views — 3-Tier Layer, Metadata Extensions & Access Control

1. **Basic Layer (`Z_B_`):** Reads directly from DB tables. One source table per view. No JOINs.
2. **Interface Layer (`Z_I_`):** Composes data via JOINs and associations. Never reads DB tables directly.
3. **Consumption Layer (`Z_C_`):** Exposes data via OData / Fiori UI.
4. **Metadata Extensions (`.ddlx`):** Separate UI annotations (`@UI.lineItem`, `@UI.identification`) from CDS views using `@Metadata.allowExtensions: true`.
5. **Access Control (DCL):** Implement row-level security using Data Control Language roles (`DEFINE ROLE Z_R_...`).

---

## Naming Conventions

### Type Definitions (`TYPES`)
- **Structures:** `tys_` (e.g., `tys_header`)
- **Internal Tables:** `tyt_` (e.g., `tyt_items`)

### Variables & Constants (`<scope><type>_<name>`)
- **Scope:** `l` (Local), `g` (Global), `a` (Attribute)
- **Type:** `v` (Value), `s` (Structure), `t` (Table), `o` (Object)
- **Constants:** `cv_...`, `cs_...`
- **Parameters:** `iv_` (Importing), `ev_` (Exporting), `cv_` (Changing), `rv_` (Returning)

---

## ABAP Unit Testing

Write automated unit tests for all domain logic classes:
```abap
CLASS lcl_test DEFINITION FINAL FOR TESTING
  DURATION SHORT
  RISK LEVEL HARMLESS.
  
  PRIVATE SECTION.
    METHODS test_calculation FOR TESTING RAISING cx_static_check.
ENDCLASS.

CLASS lcl_test IMPLEMENTATION.
  METHOD test_calculation.
    DATA(lo_cut) = NEW zcl_business_logic( ).
    DATA(lv_result) = lo_cut->_calculate( 10 ).
    cl_abap_unit_assert=>assert_equals(
      act = lv_result
      exp = 20
      msg = |Calculation failed| ).
  ENDMETHOD.
ENDCLASS.
```

---

## Modern Syntax & Anti-Patterns

| Anti-Pattern | Modern / Recommended Approach |
| :--- | :--- |
| `CONCATENATE` statements | String templates: `|{ a } { b }|` |
| Pre-declaring variables before `SELECT` | Inline declarations: `SELECT SINGLE * FROM ... INTO @DATA(ls_data)` |
| `SELECT ... ENDSELECT` / `UP TO 1 ROWS` | `SELECT ... INTO TABLE @DATA(lt_data)` + `CHECK sy-subrc = 0 and lines( lt_data ) > 0` + `lt_data[ 1 ]` |
| `IF sy-subrc <> 0. RETURN. ENDIF.` | `CHECK sy-subrc = 0.` |
| `LOOP AT lt_data INTO ls_row` | `LOOP AT lt_data ASSIGNING FIELD-SYMBOL(<fs_row>)` |
