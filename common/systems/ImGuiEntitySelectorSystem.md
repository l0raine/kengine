# [ImGuiEntitySelectorSystem](ImGuiEntitySelectorSystem.hpp)

`System` that renders an ImGui window that lets users search for and select `Entities` (which will attach a [SelectedComponent](../components/SelectedComponent.hpp) to them).

To make a `Component` be taken into account in the search, it must first be registered by calling [registerComponentEditor](../helpers/RegisterComponentEditor.hpp), [registerComponentFunctions](../helpers/RegisterComponentFunctions.hpp) and [registerComponentMatcher](../helpers/RegisterComponentMatcher.hpp).