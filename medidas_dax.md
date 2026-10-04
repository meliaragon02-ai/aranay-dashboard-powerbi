# Medidas DAX: Aranay Dashboard

Todas las medidas están en la tabla `tb_ventas_consultas`.

```DAX
Consultas =
CALCULATE ( COUNTROWS ( tb_ventas_consultas ), tb_ventas_consultas[Tipo] = "Consulta" )

Ventas =
CALCULATE ( COUNTROWS ( tb_ventas_consultas ), tb_ventas_consultas[Tipo] = "Venta" )

Tasa de conversión =
DIVIDE ( [Ventas], [Consultas] )

Facturación =
CALCULATE ( SUM ( tb_ventas_consultas[Monto ($)] ), tb_ventas_consultas[Tipo] = "Venta" )

Ticket promedio =
DIVIDE ( [Facturación], [Ventas] )

Facturación a precio de lista =
CALCULATE (
    SUMX (
        tb_ventas_consultas,
        tb_ventas_consultas[Cantidad] * RELATED ( tb_productos[Precio de lista web ($)] )
    ),
    tb_ventas_consultas[Tipo] = "Venta"
)

Costo de descuentos =
[Facturación a precio de lista] - [Facturación]
```

## Modelo de datos

- `tb_productos[Producto]` (1) → `tb_ventas_consultas[Producto]` (*), filtro en una sola dirección.
- Las tablas de Instagram (`tb_cuenta`, `tb_formatos`, `tb_contenido`, `tb_horaria`, `tb_embudo`) son independientes.
- La página "Escenario comercial" tiene un filtro de página `Origen = "Simulado"`.
