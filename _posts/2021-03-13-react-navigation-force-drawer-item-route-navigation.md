---
title: 🚀React Native🚀 - react-navigation - force drawer item route navigation
---

[Github markdown version - can be easier to read](https://github.com/yerevin/blog/blob/gh-pages/_posts/2021-03-13-react-navigation-force-drawer-item-route-navigation.md)

The main point is to overwrite navigation property passed to DrawerItemList

  navigation={{
    dispatch: (event: any) => {
      const routeName = event?.payload?.name;
      if (routeName) {
        return props.navigation.reset({
          index: 0,
          routes: [{ name: routeName }]
       });
      }

      return props.navigation.closeDrawer();
    }
  }}

Below is for example whole one file you can hold it

```typescript
const FirstScreen = () => <View/>
const SecondScreen = () => <View/>
```

```typescript
const FirstRouteStackNavigator = () => (
  <Stack.Navigator>
    <Stack.Screen
      name="Screen 1"
      component={FirstScreen}
    />
  </Stack.Navigator>
);

const SecondRouteStackNavigator = () => (
  <Stack.Navigator>
    <Stack.Screen
      name="Screen 2"
      component={SecondScreen}
    />
  </Stack.Navigator>
);
```

```typescript
function CustomDrawerContent(props) {
  return (
    <DrawerContentScrollView {...props}>
      <View>
        <DrawerItemList
          {...props}
          navigation={{
            dispatch: (event: any) => {
              const routeName = event?.payload?.name;
              if (routeName) {
                return props.navigation.reset({
                  index: 0,
                  routes: [{ name: routeName }]
                });
              }

              return props.navigation.closeDrawer();
            }
          }}
        />
      </View>
    </DrawerContentScrollView>
  );
}
```

```typescript
const Drawer = createDrawerNavigator();

const DrawerNavigator = () => {
  return (
    <Drawer.Navigator
      initialRouteName="Screen 1"
      drawerContent={(props) => <CustomDrawerContent {...props} />}
    >
      <Drawer.Screen name="Screen 1" component={FirstRouteStackNavigator} />
      <Drawer.Screen name="Screen 2" component={SecondRouteStackNavigator} />
    </Drawer.Navigator>
  );
};
```
