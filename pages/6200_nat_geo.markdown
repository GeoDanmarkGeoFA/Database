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

<h1>{{ theme }}</h1>

<h2>Generelle felter</h2>

<table class="table table-striped">
    <thead>
    <tr>
        <th>Feltnavn</th>
        <th>Feltnavn10</th>
        <th>Formål</th>
        <th>Datatype</th>
        <th>Værdiområde</th>
        <th>Obligatorisk</th>
    </tr>
    </thead>
    <tbody>
    {% for column in generel.columns %}
        <tr {% if column.name == "versions_id" or column.name == "objekt_id" or column.name == "systid_fra" or column.name == "systid_til" or column.name == "oprettet" %} style="background-color: pink" {% endif %}>
            {% assign checks = column._checks[0] %}
            {%- assign allow_null = column.is_nullable %}
            {% if column.is_nullable %}
                {% assign must = false %}
            {% else %}
                {% assign must = true %}
            {% endif %}

            {% if meta.fields[column.name].restriction != null %}
                {% assign rs = meta.fields[column.name].restriction %}
                {% capture restrictions %}
                    {% if meta.fields[column.name].restriction != null %}{% for r in rs %} {{ r.value }} = {{ r.alias }}
                        <br>  {% endfor %}{% endif %}
                {% endcapture %}
            {% else %}
                {% assign restrictions = null %}
            {% endif %}

            <td>{{ column.name }}</td>
            <td>10 tegn</td>
            <td>{{ column.comment }}</td>
            <td>{{ column.type }}</td>
            <td>{{ restrictions | truncate: 1000 }}{{ checks }}</td>
            <td>{{ must }}</td>
        </tr>

    {% endfor %}
    </tbody>
</table>

<h2 class="mt-5">Felter i {{ theme }}</h2>

<table class="table table-striped">
    <thead>
    <tr>
        <th>Feltnavn</th>
        <th>Feltnavn10</th>
        <th style="width: 400px">Formål</th>
        <th>Datatype</th>
        <th>Værdiområde</th>
        <th>Obligatorisk</th>
    </tr>
    </thead>
    <tbody>
    {% for column in table.columns %}
        <tr {% if column.name == "versions_id" %} style="background-color: pink" {% endif %}>
            {% assign checks = column._checks[0] %}
            {% assign comment = meta.fields[column.name].comment %}

            {% if column.is_nullable %}
                {% assign must = false %}
            {% else %}
                {% assign must = true %}
            {% endif %}

            {% if meta.fields[column.name].restriction != null %}
                {% assign rs = meta.fields[column.name].restriction %}
                {% capture restrictions %}
                    {% if meta.fields[column.name].restriction != null %}{% for r in rs %} {{ r.value }} = {{ r.alias }}
                        <br>  {% endfor %}{% endif %}
                {% endcapture %}
            {% else %}
                {% assign restrictions = null %}
            {% endif %}

            <td>{{ column.name }}</td>
            <td>10 tegn</td>
            <td>{{ comment }}</td>
            <td>{{ column.type }}</td>
            <td>{{ restrictions }}{{ checks }}</td>
            <td>{{ must }}</td>
        </tr>
    {% endfor %}
    </tbody>
</table>

