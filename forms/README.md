# PDFs oficiales del IRS (templates AcroForm)

Coloca aquí los PDFs **rellenables** oficiales del IRS. Esta carpeta es
`public/forms/`, servida en la raíz del sitio como `/forms/...`. Sin estos
archivos, SubPay genera un PDF de respaldo simplificado (funciona, pero no es la
forma oficial).

| Archivo                | Fuente                                                          | Usado por                |
| ---------------------- | -------------------------------------------------------------- | ------------------------ |
| `fw9_2024.pdf`         | https://www.irs.gov/pub/irs-pdf/fw9.pdf (Rev. 2024)            | `src/lib/pdf/w9.ts`      |
| `f1099nec_2025.pdf`    | https://www.irs.gov/pub/irs-pdf/f1099nec.pdf (año fiscal 2025) | `src/lib/pdf/nec1099.ts` |
| `f1099nec_2026.pdf`    | versión 2026 cuando el IRS la publique (Box 1→1a, tips/overtime) | `src/lib/pdf/nec1099.ts` |

## Cómo descargarlos

```sh
cd public/forms
curl -L -o fw9_2024.pdf      https://www.irs.gov/pub/irs-pdf/fw9.pdf
curl -L -o f1099nec_2025.pdf https://www.irs.gov/pub/irs-pdf/f1099nec.pdf
```

## Verificar / ajustar nombres de campo AcroForm

Los nombres de campo del IRS son largos (p.ej.
`topmostSubform[0].Page1[0].f1_01[0]`) y cambian por revisión. Los fillers usan
listas de candidatos (`setTextFlexible`). Si un campo no se rellena, lista los
nombres reales con `pdf-lib`:

```js
import { PDFDocument } from "pdf-lib";
const doc = await PDFDocument.load(await fetch("/forms/fw9_2024.pdf").then((r) => r.arrayBuffer()));
doc.getForm().getFields().forEach((f) => console.log(f.getName()));
```

Luego añade el nombre correcto al inicio del array de candidatos en
`w9.ts` / `nec1099.ts`.

> ⚠️ La copia roja **Copy A** del 1099 es escaneable por el IRS y **no** debe
> imprimirse desde aquí. SubPay genera solo la **copia del destinatario (Copy B)**.
