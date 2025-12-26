Kotlin
‎package com.evansrana.xplan.ui
‎
‎import androidx.compose.material.*
‎import androidx.compose.runtime.Composable
‎import androidx.navigation.NavHostController
‎import androidx.navigation.compose.*
‎import androidx.compose.material.icons.Icons
‎import androidx.compose.material.icons.filled.Home
‎import androidx.navigation.compose.composable
‎import com.evansrana.xplan.ui.screens.DeviceListScreen
‎import com.evansrana.xplan.ui.screens.VoiceScreen
‎import com.evansrana.xplan.ui.screens.SettingsScreen
‎
‎@Composable
‎fun AppNavHost() {
‎    val navController = rememberNavController()
‎    val items = listOf("devices", "voice", "settings")
‎
‎    Scaffold(
‎        bottomBar = {
‎            BottomNavigation {
‎                items.forEach { route ->
‎                    BottomNavigationItem(
‎                        selected = navController.currentDestination?.route == route,
‎                        onClick = { navController.navigate(route) },
‎                        icon = { Icon(Icons.Default.Home, contentDescription = null) },
‎                        label = { Text(route.capitalize()) }
‎                    )
‎                }
‎            }
‎        }
‎    ) {
‎        NavHost(
‎            navController = navController,
‎            startDestination = "devices"
‎        ) {
‎            composable("devices") { DeviceListScreen(navController) }
‎            composable("voice") { VoiceScreen(navController) }
‎            composable("settings") { SettingsScreen(navController) }
‎        }
‎    }
‎}
‎2️⃣ DeviceListScreen.kt (new file)
‎Save in: app/src/main/java/com/evansrana/xplan/ui/screens/DeviceListScreen.kt
‎Copy code
‎Kotlin
‎package com.evansrana.xplan.ui.screens
‎
‎import androidx.compose.foundation.layout.*
‎import androidx.compose.foundation.lazy.*
‎import androidx.compose.material.*
‎import androidx.compose.runtime.*
‎import androidx.compose.ui.Modifier
‎import androidx.compose.ui.unit.dp
‎import androidx.navigation.NavController
‎import com.evansrana.xplan.data.Device
‎import com.evansrana.xplan.data.MockDeviceRepository
‎
‎@Composable
‎fun DeviceListScreen(navController: NavController) {
‎    val repo = MockDeviceRepository()
‎    val devices = remember { repo.getDevices() }
‎    var states by remember { mutableStateOf(devices.associate { it.id to false }) }
‎
‎    LazyColumn(modifier = Modifier.fillMaxSize().padding(16.dp)) {
‎        item {
‎            Text("Smart Devices", style = MaterialTheme.typography.h5)
‎            Spacer(Modifier.height(12.dp))
‎        }
‎        items(devices) { device ->
‎            Card(
‎                modifier = Modifier.fillMaxWidth().padding(vertical = 6.dp),
‎                elevation = 3.dp
‎            ) {
‎                Row(
‎                    Modifier.padding(12.dp),
‎                    horizontalArrangement = Arrangement.SpaceBetween
‎                ) {
‎                    Text(device.name)
‎                    Switch(
‎                        checked = states[device.id] == true,
‎                        onCheckedChange = {
‎                            states = states.toMutableMap().apply { put(device.id, it) }
‎                        }
‎                    )
‎                }
‎            }
‎        }
‎    }
‎}
‎3️⃣ MockDeviceRepository.kt (keep if not already saved)
‎Location: app/src/main/java/com/evansrana/xplan/data/MockDeviceRepository.kt
‎Copy code
‎Kotlin
‎package com.evansrana.xplan.data
‎
‎data class Device(val id: String, val name: String)
‎
‎class MockDeviceRepository {
‎    fun getDevices(): List<Device> = listOf(
‎        Device("1", "Living Room Speaker"),
‎        Device("2", "Bedroom Light")
‎    )
‎}
‎