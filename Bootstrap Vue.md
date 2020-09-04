###### Bootstrap Vue

借助BootstrapVue，您可以使用Vue.js和世界上最受欢迎的前端CSS库Bootstrap v4在网络上构建响应式，移动优先和ARIA可访问的项目。

Bootstrap v4是世界上最受欢迎的框架，用于构建响应式，移动优先的网站。

Vue.js是用于构建用户界面的渐进框架。

BootstrapVue 拥有85个以上的组件，45个以上的可用插件，多个指令和1000多个图标，它提供了可用于Vue.js v2.6的Bootstrap v4 组件和网格系统的最全面的实现之一，并具有广泛而自动化的WAI-ARIA可访问性标记。

###### 选择BootstrapVue的理由

反应灵敏：移动优先响应式布局

模块化的：仅导入所需的功能

无障碍：自动化的WAI-ARIA可访问性标记

现代：使用Vue.js v2.6和Bootstrap SCSS v4.5构建

可配置的：使用SCSS变量和全局选项 创建主题

自由： 开源于GitHub, MIT许可证

###### HTML5文件类型

Bootstrap需要使用HTML5文档类型。没有它，您会看到一些时髦的不完整样式，但是包括它不会引起任何可观的困扰。

###### 响应式元标记

引导开发*移动首先*，在我们的移动设备首先优化的代码，然后扩展组件需要使用CSS媒体查询的战略。为确保所有设备都能正确渲染和触摸缩放，**请将响应式视口meta标签添加**到中`<head>`。

```
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

###### 装箱尺寸

为了更简单地调整CSS大小，我们将全局`box-sizing`值从切换`content-box`为`border-box`。这样可以确保`padding`不会影响元素的最终计算宽度，但是会导致某些第三方软件（例如Google Maps和Google Custom Search Engine）出现问题。

```
box-sizing: content-box|border-box|inherit;
```

| 值          | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| content-box | 这是由CSS2.1规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。 |
| border-box  | 为元素设定的宽度和高度决定了元素的边框盒。就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。 |
| inherit     | 规定应从父元素继承box-sizing属性的值。                       |

容器是Bootstrap中最基本的布局元素。在使用我们的默认网格系统时，容器是必需的。容器用于在其中容纳，填充和（有时）使内容居中。尽管可以嵌套容器，但是大多数布局都不需要嵌套容器。

Bootstrap带有三个不同的容器：

- .container，它max-width在每个响应断点处设置一个
- .container-fluid，这是width:100%所有断点
- .container-{breakpoint}, width:100%直到指定的断点为止

默认.container类是响应式，固定宽度的容器，这意味着它max-width在每个断点处都会更改。

v4不再支持IE8，IE9和iOS 6。v4现在仅是IE10 +和iOS 7+。对于需要这两个网站之一的网站，请使用v3。

###### 全球变化(V4)

- **默认情况下，Flexbox已启用。**总的来说，这意味着要远离浮标，而要跨越我们的组件。
- 将源CSS文件从[Less](http://lesscss.org/)切换到[Sass](https://sass-lang.com/)。
- 从`px`切换`rem`为我们的主要CSS单位，尽管像素仍然用于媒体查询和网格行为，因为设备视口不受类型大小的影响。
- 全局字体大小从`14px`增加到`16px`。
- 改进了网格层以添加第五个选项（寻址以下的较小设备`576px`），并`-xs`从这些类中删除了中缀。范例：`.col-6.col-sm-4.col-md-3`。
- 通过SCSS变量（例如`$enable-gradients: true`）将单独的可选主题替换为可配置选项。
- 修改了构建系统，以使用一系列npm脚本代替Grunt。有关`package.json`所有脚本的信息，请参见，或者针对本地开发需求的项目自述文件。
- 不再支持Bootstrap的非响应式使用。
- 放弃在线定制程序，转而使用更广泛的设置文档和定制版本。
- 为常见的CSS属性值对和边距/填充间距快捷方式添加了数十个新的[实用程序类](https://getbootstrap.com/docs/4.5/utilities/)。



###### BootstrapVue 组件

1.Alert  <b-alert>

Importing individual components

You can import individual components into your project via the following named exports:

| Component | Named Export | Import Path   |
| --------- | ------------ | ------------- |
| <b-alert> | BAlert       | bootstrap-vue |

Example:

```
import {BAlert} from 'bootstrap-vue';
Vue.component('b-alert', BAlert);
```

Importing as a Vue.js plugin

This plugin includes all of the above listed individual components.Plugins also include any component aliases.

| Named Export | Import Path   |
| ------------ | ------------- |
| AlertPlugin  | bootstrap-vue |

Example:

```
import {AlertPlugin} from 'bootstrap-vue';
Vue.use(AlertPlugin);
```

2.Aspect <b-aspect>

3.Avatar <b-avatar> <a-avatar-group>

4.Badge <b-badge>

5.Breadcrumb <b-breadcrumb> <b-breadcrumb-item>

6.Button <b-button> aliases <b-btn>  

​				 <b-button-close> aliases <b-btn-close>

7.Button Group <b-button-group> aliases <b-btn-group>

8.Button Toolbar <b-button-toolbar> aliases <b-btn-toolbar>

9.Calendar <b-calendar>

10.Card  <b-card> <b-card-header> <b-card-footer> <b-card-body> <b-card-title>

<b-card-sub-title>  <b-card-img>  <b-card-img-lazy> <b-card-text> <b-card-group>

11.Carousel  轮播图 <b-carousel>

12.Collapse 折叠 <b-collapse>

13.Dropdown 下拉菜单 <b-dropdown> supported  sub-components <b-dropdown-item>  <b-dropdown-item-button> <b-dropdown-divider>  <b-dropdown-text>  <b-dropdown-form> <b-dropdown-group> <b-dropdown-header>

14.Embed <b-embed>

15.Form <b-form>

<b-form-input>  <b-form-textarea> <b-form-select> <b-form-radio> <b-form-checkbox>  <b-form-file> <b-form-datepicker> <b-form-spinbutton>  <b-form-tags>  <b-form-timepicker>  <b-form-rating> <b-button> <b-form-group> <b-input-group> <b-form-row>

16.Form Checkbox <b-form-checkbox>  <b-form-checkbox-group>

<b-form-checkbox-group> aliases <b-checkbox-group>  <b-check-group>

<b-form-checkbox> aliases <b-checkbox>  <b-check>

17.Form Datepicker <b-form-datepicker> aliases <b-datepicker>

18.Form File  上传 <b-form-file> ailases <b-file>

19.Form Group <b-form-group>

20.Form Input <b-form-input> type: text、number、email、password、search、url、tel、date、time、range、color

<b-form-input> aliases <b-input>

21.Form Radio  <b-form-radio-group> <b-form-radio>

<b-form-radio-group> aliases <b-radio-group>

<b-form-radio> aliases <b-radio>

22.Form Rating 打分 <b-form-rating> aliases <b-rating>

23.Form Select  <b-form-select> aliases <b-select>

24.Form Spinbutton 自定义数字范围形式控制 <b-form-spinbutton> aliases <b-spinbutton>

25.Form Tags <b-form-tags> aliases <b-tags>

26.Form Textarea <b-form-textarea> aliases <b-textarea>

27.Form Timepicker <b-form-timepicker> aliases <b-timepicker>

28.Image <b-img> <b-img-lazy>

29.Input Group 在文本输入的两边添加文本、按钮或按钮组 <b-input-group> <b-input-group-prepend> <b-input-group-append> <b-input-group-text> <b-input-group-addon>

30.Jumbotron <b-jumbotron>

31.Layout and Grid System Flexbox的网格 <b-container> <b-row> <b-form-row> <b-col>

12列布局

Extra small（xs）、Small（sm）、Medium（md）、Large（lg）、Extra large（xl）

<576px                      >=576px           >=768px                 >=992px           >=1200px

cols="*"                      sm="*"                  md='*'                    lg='*'                        xl=''

 32.Link  <b-link>

33.List group <b-list-group> <b-list-group-item>

34.Media <b-media>  <b-media-aside>  <b-media-body> 

35.Modal  对话提示框 <b-modal>

36.Nav  导航 <b-nav> <b-nav-item> <b-nav-text> <b-nav-form> <b-nav-item-dropdown>

<b-nav-item-dropdown> aliases <b-nav-item-dd> 、<b-nav-dropdown> 、<b-nav-dd>

37.Navbar 导航栏 <b-navbar> 支持 <n-navbar-brand> <b-navbar-toggle> <b-collapse is-nav>  <b-navbar-nav> 

<b-navbar-nav>z支持<b-nav-item> <b-nav-item-dropdown> <b-nav-text> <b-nav-form>

38.Overlay 覆盖 <b-overlay>

39.Pagination 分页 <b-pagination>

40.Pagination Nav 分页导航 <b-pagination-nav>

41.Popover 弹窗 <b-popover>

42.Progress 进度条 <b-progress>  <b-progress-bar>

43.Sidebar 侧边栏 <b-sidebar>

44.Spinner 加载状态 loading <b-spinner>

45.Table <b-table>   <b-tbody> <b-thead> <b-tfoot> <b-tr> <b-td> <b-th>

<b-table-lite> <b-table-simple>  轻巧的替代组件

46.Tab 标签 <b-tab>

47.Time 时间 <b-time>

48.Toast  显示通知 <b-toast> <b-toaster>

49.Tooltip 提示信息 <b-tooltip>

`v-model` is optionally supported (updated by the `input` event, and tied to the `value` prop). 

指令：

1.<v-b-hover>  一种轻量型指令，允许您在元素悬停或未悬停时做出反应

2.<v-b-popover> 弹出窗口是类的工具提示。

3.<v-b-scrollspy>根据滚动位置自动激活导航或列出组组件，以指示当前在视口中处于活动状态的链接

4.<v-b-toggle>轻型指令，用于通过ID切换折叠和侧边栏的可见状态。它会自动处理触发器元素上的可访问性属性

5.<v-b-tooltip>将自定义BootstrapVue工具提示添加到任何元素。可以通过鼠标悬停，聚焦或单击某个元素来触发工具提示。

6.<v-b-visible> 当元素在视口中可见时，v-b-visible指令允许您做出反应

class='mb-0' ,什么意思？

Bootstrap具有广泛的响应边距和填充实用程序类。它们适用于所有断点：

xs(<=576px) sm(>=576px) md(>=768px) lg(>=992px)  xl(>=1200px)

m -- margin

p- - padding

t --设置margin-top 或padding-top

b--设置margin-bottom或padding-bottom

l--设置margin-left 或 padding-left

r--设置margin-right 或padding-right

x--设置padding-left和padding-right或margin-left和margin-right

y--设置padding-top和padding-bottom 或 margin-top和margin-bottom

空白 -- 在元素的所有4个边上设置边距或填充

0--将边距或填充设置为0

1--将边距或填充设置为.25rem(如果font-size为16px则为4px)

2--将边距或填充设置为.5rem(如果font-size为16px则为8px)

3--将边距或填充设置为1rem(如果font-size为16px则为16px)

4--将边距或填充设置为1.5rem(如果font-size为16px则为24px)

5--将边距或填充设置为3rem(如果font-size为16px则为48px)

自动--将设置为自动

