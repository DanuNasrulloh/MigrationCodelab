# Migrating to Jetpack Compose

Code lab 6 - Composable from xml
<br/>
<img align="right" width="200" height="500" alt="image" src="https://github.com/user-attachments/assets/25d3770f-206b-454a-b1c3-6870ce048713" />
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
Code lab 7 - view model and live data
<img align="right" width="206" height="158" alt="image" src="https://github.com/user-attachments/assets/1ec910ea-cf25-40c9-ae2c-f4ea5821a0ce" />

```
Code :
@Composable
fun PlantDetailDescription(plantDetailViewModel: PlantDetailViewModel) {
    // Observes values coming from the VM's LiveData<Plant> field
    val plant by plantDetailViewModel.plant.observeAsState()

    // If plant is not null, display the content
    plant?.let {
        PlantDetailContent(it)
    }
}

@Composable
fun PlantDetailContent(plant: Plant) {
    PlantName(plant.name)
}

@Preview
@Composable
private fun PlantDetailContentPreview() {
    val plant = Plant("id", "Avocado", "description", 3, 30, "")
    MaterialTheme {
        PlantDetailContent(plant)
    }
}
```
Code lab 8 - view model and live data
<img align="right" width="205" height="160" alt="image" src="https://github.com/user-attachments/assets/090b9001-efa9-45ab-b80e-685dde527938" />
```
Code : @OptIn(ExperimentalComposeUiApi::class)
@Composable
private fun PlantWatering(wateringInterval: Int) {
    Column(Modifier.fillMaxWidth()) {
        // Same modifier used by both Texts
        val centerWithPaddingModifier = Modifier
            .padding(horizontal = dimensionResource(R.dimen.margin_small))
            .align(Alignment.CenterHorizontally)

        val normalPadding = dimensionResource(R.dimen.margin_normal)

        Text(
            text = stringResource(R.string.watering_needs_prefix),
            color = MaterialTheme.colorScheme.primaryContainer,
            fontWeight = FontWeight.Bold,
            modifier = centerWithPaddingModifier.padding(top = normalPadding)
        )

        val wateringIntervalText = pluralStringResource(
            R.plurals.watering_needs_suffix, wateringInterval, wateringInterval
        )
        Text(
            text = wateringIntervalText,
            modifier = centerWithPaddingModifier.padding(bottom = normalPadding)
        )
    }
}

@Preview
@Composable
private fun PlantWateringPreview() {
    MaterialTheme {
        PlantWatering(7)
    }
}
```
Code lab 9 - view in compose Code
<img align="right" width="200" height="500" alt="Screenshot_20251003_211027" src="https://github.com/user-attachments/assets/a6adf0f8-dd46-4e5b-8ef1-addd6d9a0122" />

```
@Composable
private fun PlantDescription(description: String) {
    // Remembers the HTML formatted description. Re-executes on a new description
    val htmlDescription = remember(description) {
        HtmlCompat.fromHtml(description, HtmlCompat.FROM_HTML_MODE_COMPACT)
    }

    // Displays the TextView on the screen and updates with the HTML description when inflated
    // Updates to htmlDescription will make AndroidView recompose and update the text
    AndroidView(
        factory = { context ->
            TextView(context).apply {
                movementMethod = LinkMovementMethod.getInstance()
            }
        },
        update = {
            it.text = htmlDescription
        }
    )
}

@Preview
@Composable
private fun PlantDescriptionPreview() {
    MaterialTheme {
        PlantDescription("HTML<br><br>description")
    }
}
```
Code lab 10 - view in compose Code
```
Code : 
 composeView.apply {
                setViewCompositionStrategy(
                    ViewCompositionStrategy.DisposeOnViewTreeLifecycleDestroyed
                )
                setContent {
                    MaterialTheme {
                        PlantDetailDescription(plantDetailViewModel)
                    }
                }
            }
```
Code lab 11 - view in compose Code

```
@Composable
fun SunflowerTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    content: @Composable () -> Unit
) {
    val lightColors  = lightColorScheme(
        primary = colorResource(id = R.color.sunflower_green_500),
        primaryContainer = colorResource(id = R.color.sunflower_green_700),
        secondary = colorResource(id = R.color.sunflower_yellow_500),
        background = colorResource(id = R.color.sunflower_green_500),
        onPrimary = colorResource(id = R.color.sunflower_black),
        onSecondary = colorResource(id = R.color.sunflower_black),
    )
    val darkColors  = darkColorScheme(
        primary = colorResource(id = R.color.sunflower_green_100),
        primaryContainer = colorResource(id = R.color.sunflower_green_200),
        secondary = colorResource(id = R.color.sunflower_yellow_300),
        onPrimary = colorResource(id = R.color.sunflower_black),
        onSecondary = colorResource(id = R.color.sunflower_black),
        onBackground = colorResource(id = R.color.sunflower_black),
        surface = colorResource(id = R.color.sunflower_green_100_8pc_over_surface),
        onSurface = colorResource(id = R.color.sunflower_white),
    )
    val colors = if (darkTheme) darkColors else lightColors
    MaterialTheme(
        colorScheme = colors,
        content = content
    )
}
```
Code lab 12 - view in compose Code
<img align="right" width="1728" height="963" alt="image" src="https://github.com/user-attachments/assets/2b21e8b4-304b-4953-a045-1f8b21b4cda7" />
```
composeTestRule.activityRule.scenario.onActivity { gardenActivity ->
            activity = gardenActivity

            val bundle = Bundle().apply { putString("plantId", "malus-pumila") }
            findNavController(activity, R.id.nav_host).navigate(R.id.plant_detail_fragment, bundle)
```
