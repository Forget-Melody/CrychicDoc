# 盔甲类物品

- 本文旨在创建具有与原版盔甲类物品具有相同功能的物品，以实现快照25w31a的铜头盔为例。

## 注册物品

- 在startup_scripts内编写脚本注册物品。
- 注册一个名为'kubejs:copper_helmet'的头盔物品，设置tier为锁链等级，并修改修复原料为'#forge:ingots/copper'，设置tier名为copper，tier名将用于minecraft寻找盔甲贴图。
- kubejs无法实现ArmorTier接口，因此需要选定1个参考tier，并通过modifyTier修改属性。

::: code-group

```js [KubeJS]
StartupEvents.registry('minecraft:item', event => {
    event.create('kubejs:copper_helmet', 'helmet')
    .tier($ArmorMaterials.CHAIN)
    .modifyTier(tier => {
      tier.setRepairIngredient('#forge:ingots/copper');
      tier.setName('kubejs:copper');
    })
    .translationKey('item.kubejs.copper_helmet');
})
```

:::

## 物品纹理

- 将物品纹理放置在kubejs/assets/textures/item/目录，命名为copper_helmet.png

## 物品模型

- 创建物品模型，在kubejs/assets/models/item目录下创建copper_helmet.json。
- 需要为每种质地的纹理创建模型。
- textures标识盔甲物品本身的纹理id。
- overrides标识不同锻造模板类型下使用的模型id。

::: code-group

```json 
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "kubejs:item/copper_helmet"
  },
  "overrides": [
    {
      "model": "kubejs:item/copper_helmet_quartz_trim",
      "predicate": {
        "trim_type": 0.1
      }
    },
    {
      "model": "kubejs:item/copper_helmet_iron_trim",
      "predicate": {
        "trim_type": 0.2
      }
    },
    {
      "model": "kubejs:item/copper_helmet_netherite_trim",
      "predicate": {
        "trim_type": 0.3
      }
    },
    {
      "model": "kubejs:item/copper_helmet_redstone_trim",
      "predicate": {
        "trim_type": 0.4
      }
    },
    {
      "model": "kubejs:item/copper_helmet_copper_trim",
      "predicate": {
        "trim_type": 0.5
      }
    },
    {
      "model": "kubejs:item/copper_helmet_gold_trim",
      "predicate": {
        "trim_type": 0.6
      }
    },
    {
      "model": "kubejs:item/copper_helmet_emerald_trim",
      "predicate": {
        "trim_type": 0.7
      }
    },
    {
      "model": "kubejs:item/copper_helmet_diamond_trim",
      "predicate": {
        "trim_type": 0.8
      }
    },
    {
      "model": "kubejs:item/copper_helmet_lapis_trim",
      "predicate": {
        "trim_type": 0.9
      }
    },
    {
      "model": "kubejs:item/copper_helmet_amethyst_trim",
      "predicate": {
        "trim_type": 1.0
      }
    }
  ]
}
```

::: 

- 创建有纹饰的铜头盔模型，以石英质为例
- layer0标识了铜头盔的纹理id
- layer0标识了石英质物品纹理id

::: code-group

```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "kubejs:item/copper_helmet",
    "layer1": "minecraft:trims/items/helmet_trim_quartz"
  }
}
```

:::

## 盔甲纹理

- 将准备好的盔甲纹理放在kubejs/assets/textures/models/armor/目录下，分别命名为copper_layer_1与copper_layer_2（可参考minecraft同目录下的贴图）。
- 不需要为每种纹饰准备盔甲纹理。

## 物品标签

- 在server_scripts内编写脚本，为刚刚创建的盔甲物品添加标签。
- 除盔甲类物品的共有标签外，每个部位的盔甲物品还有各自的标签。

::: code-group

```js [KubeJS]
ServerEvents.tags('minecraft:item', event => {
  event.add('minecraft:trimmable_armor', 'kubejs:copper_helmet')
  event.add('forge:armors', 'kubejs:copper_helmet')
    event.add('forge:armors/helmets', 'kubejs:copper_helmet')
})

```

:::

## 物品配方

- 在server_scripts内编写脚本，为刚刚创建的铜头盔添加有序配方。
- 这里使用了forge铜锭标签。

::: code-group

```js [KubeJS]
ServerEvents.recipes(event => {
    event.recipes.minecraft.crafting_shaped('kubejs:copper_helmet', [
    'AAA',
    'A A',
    '   '
  ], {
    'A': '#forge:ingots/copper'
  })
})
```

:::