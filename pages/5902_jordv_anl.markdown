---
---

{% assign theme = page.name | split: "." | first %}

<!-- Build your "meta" key dynamically -->
{% assign metaKey = "fkgt_" | append: theme %}
{% assign meta = site.data.meta[metaKey] %}

<!-- "generel" can still be static if that never changes -->
{% assign generel = site.data.fkg.tables.generel %}

<!-- Build your "table" key dynamically, e.g. "t_5612_vinterserviceomraade_t" -->
{% assign tableKey = "t_" | append: theme | append: "_t" %}
{% assign table = site.data.fkg.tables[tableKey] %}

<style>
    @media print {
        -webkit-print-color-adjust: exact;
    }
    .container-lg {
        max-width: none !important;
    }
</style>

<h1>{{ theme }}</h1>

<h2>Metadata</h2>
<table>
    <thead>
    <tr>
        <th>Metadataelement</th>
        <th>Beskrivelse</th>
    </tr>
    </thead>

    <tbody>
    <tr>
        <td>Temanavn</td>
        <td>{{ meta.meta.temanavn }}</td>
    </tr>
    <tr>
        <td>Temakode</td>
        <td>{{ meta.meta.temakode }}</td>
    </tr>
    <tr>
        <td>Formål</td>
        <td>{{ meta.meta.formaal }}</td>
    </tr>
    <tr>
        <td>Definition</td>
        <td>{{ meta.meta.definition }}</td>
    </tr>
    <tr>
        <td>Beskrivelse</td>
        <td>{{ meta.meta.beskrivelse }}</td>
    </tr>

    <tr>
        <td>Nøgleord hovedgruppe</td>
        <td>{{ meta.meta.noegleord_hovedgruppe }}</td>
    </tr>
    <tr>
        <td>Nøegleord</td>
        <td>{{ meta.meta.noegle_ord }}</td>
    </tr>
    <tr>
        <td>Geometri type</td>
        <td>{{ meta.meta.geometritype }}</td>
    </tr>
    <tr>
        <td>Lovgrundlag</td>
        <td>{{ meta.meta.lovgrundlag }}</td>
    </tr>
    <tr>
        <td>KLE koder</td>
        <td>{{ meta.meta.kle_koder }}</td>
    </tr>
    </tbody>
</table>

<h2>Registreringsvejledning</h2>
<table>
    <thead>
    <tr>
        <th>Emne</th>
        <th>Beskrivelse</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>Registreringsinstruks</td>
        <td>{{ meta.meta.registreringsinstruks }}</td>
    </tr>
    <tr>
        <td>Klassificering/opdeling</td>
        <td>{{ meta.meta.klassificering_opdeling }}</td>
    </tr>
    <tr>
        <td>Minimum størrelser for objekt</td>
        <td>{{ meta.meta.minimum_stoerrelser }}</td>
    </tr>
    <tr>
        <td>Entydige objekter</td>
        <td>{{ meta.meta.entydige_objekter }}</td>
    </tr>
    <tr>
        <td>Geometrisk konsistens mellem objekter</td>
        <td>{{ meta.meta.geometrisk_konsistens }}</td>
    </tr>
    <tr>
        <td>Geometrisk konsistens med objekter i andre</td>
        <td>{{ meta.meta.geometrisk_konsistens_andre }}</td>
    </tr>
    </tbody>
</table>

<h2>Felter i udstillings-view</h2>
<div>
    Læsevejledning til specifikationen:
    <ul>
        <li>Rød farve angiver, at der er tale om generelle felter, som går igen i alle temalag.</li>
        <li>Blå farve indikerer, at der er tale om et systemudfyldt felt, hvor du ikke skal indtaste værdier. Dette er også angivet i feltet.</li>
        <li>Grøn farve indikerer, at der er tale om et kodefelt, hvor værdiområdet er begrænset af en opslagsliste. Tryk på 'Vis Liste' for at se opslagslisten.</li>
    </ul>
</div>
<table>
    <tr>
        <th>Feltnavn</th>
        <th>Feltnavn10</th>
        <th style="width: 300px">Formål</th>
        <th>Datatype</th>
        <th>Værdiområde</th>
        <th>Obligatorisk / Frit / System</th>
    </tr>

    {% for field in meta.fields %}

        {% assign column = table.columns | where: "name", field[0] %}
        {% assign g_column = generel.columns | where: "name", field[0] %}

        {% if g_column[0].comment != null %}
            {% assign comment = g_column[0].comment %}
        {% else %}
            {% assign comment = field[1].comment %}

        {% endif %}


        {% if column[0].name == field[0] %}
            {% assign system = false %}
        {% else %}
            {% assign system = true %}
        {% endif %}

        {% if column[0].is_nullable or field[0] == "noegle" or field[0] == "note" or field[0] == "systid_til" %}
            {% assign must = "F" %}
        {% else %}
            {% assign must = "O" %}
        {% endif %}

        {% if column[0]._short_name %}
            {% assign short = column[0]._short_name %}
        {% else %}
            {% assign short = field[0] %}
        {% endif %}

        {% if field[0] == "versions_id" or field[0] == "objekt_id" or field[0] == "systid_fra" or field[0] == "systid_til" or field[0] == "oprettet" or field[0] == "versions_id" or field[0] == "temakode" or field[0] == "temanavn" %}
            <tr  style="background-color: pink">
        {% elsif system and field[0] != "cvr_kode" and field[0] != "bruger_id" and field[0] != "oprindkode" and field[0] != "statuskode" and field[0] != "off_kode" and field[0] != "noegle" and field[0] != "note" %}
            <tr  style="background-color: lightblue">
            {% assign must = "S" %}
        {% elsif field[1].restriction != null %}
            <tr  style="background-color: lightgreen">
        {% else %}
            <tr>
        {% endif %}

        {% assign checks = column[0]._checks[0] %}

        {% if field[1].restriction != null %}
            {% assign restrictions = field[1].restriction %}
        {% endif %}

        {% if field[1].restriction != null %}
            {% assign rs = field[1].restriction %}
            {% capture restrictions %}
                {% if field[1].restriction != null %}{% for r in rs %} {{ r.value }} = {{ r.alias }}
                    <br>  {% endfor %}{% endif %}
            {% endcapture %}
        {% else %}
            {% assign restrictions = null %}
        {% endif %}

        <td>{{ field[0] }}</td>
        <td>{{ short }}</td>
        <td>{{ comment }}</td>
        <td>{{ field[1].full_type }}</td>
        <td>
            {% if restrictions %}
                <details>
                    <summary>Vis liste</summary>
                    {{ restrictions }}
                </details>
            {% endif %}
            {{ checks }}
        </td>
        <td>{{ must }}</td>
        </tr>
    {% endfor %}
</table>


    </tbody>
</table>


