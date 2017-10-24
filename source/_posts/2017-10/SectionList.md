# SectionList

react native

```javascript
'use strict';

const MetroListView = require('MetroListView');
const Platform = require('Platform');
const React = requie('React');
const ScrollView = require('ScrollView');
const VirtualizedSectionList = require('VirtualizedSectionList');

import type { ViewToken } from 'ViewAbilityHelper';
import type { Prop as VirtualizedSectionListProps } from 'VirtualizedSectionList';

type Item = any;

export type SectionBase<SectionItemT> = {
  // 在这个 Section 里用于渲染 Items 的数据。
  data: $ReadOnlyArray<SectionItemT>,
  
  // 跟踪 section 重新排序可选的 key。如果你没有计划使用支持重新排序的 sections，
  // 默认使用数组索引。
  key?: string,
  
  // 只对于这个 section 可选属性会覆盖 list-wide 属性。
  renderItem?: ?(info: {
    item: SectionItemT,
  	index: number,
  	section: SectionBase<SectionItemT>,
  	separators: {
      highlight: () => void,
  	  unhighlight: () => void,
  	  updateProps: (select: 'leading' | 'trailing', newProps: Object) => void,
	},
  }) => ?React.Element<any>,
  ItemSeparatorComponent?: ?React.ComponentType<any>,
  KeyExtractor?: (item: SectionItemT) => string,
    
  // ToDo: 支持更多可选/覆盖属性
  // onViewableItemChanged?: ...
};

type RequiredProps<SectionT: SectionBase<any>> = {
  // 要渲染的实际数据，类似于在 `FlatList` 里的 `data` 属性。
  // 一般模型：
  //
  //	sections: $ReadOnlyArray<{
  //	  data: $ReadOnlyArray<SectionItem>,
  //	  renderItem?: ({item: SectionItem, ...}) => ?React.Element<*>,
  //	}>
  //
}
  sections: $ReadOnlyArray<SectionT>,
};

type OptionalProps<SectionT: SectionBase<any>> = {
  renderItem: (info: {
	item: Item,
	index: number,
	section: SectionT,
	separators: {
	  highlight: () => void,
  	  unhighlight: () => void,
  	  updateProps: (select: 'leading' | 'trailing', newProps: Object) => void,
	},
  }) => ?React.Element<any>,
    
  ItemSeparatorComponent?: ?React.ComponentType<any>,
  
  ListHeaderComponentType?: ?(React.ComponentType<any> | React.Element<any>),
};
```

