# Migrating to Jetpack Compose

Code lab 6 - Composable from xml
<img width="461" height="747" alt="image" src="https://github.com/user-attachments/assets/25d3770f-206b-454a-b1c3-6870ce048713" />
```
@Composable
private fun PlantName(name: String) {
    Text(
        text = name,
        style = MaterialTheme.typography.headlineSmall,
        modifier = Modifier
            .fillMaxWidth()
            .padding(horizontal = dimensionResource(R.dimen.margin_small))
            .wrapContentWidth(Alignment.CenterHorizontally),
    )
}

@Preview
@Composable
private fun PlantNamePreview() {
    MaterialTheme {
        PlantName("Avocado")
    }
}
```
