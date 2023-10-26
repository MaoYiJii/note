# Animation

### 滑鼠事件觸發

```xml
<StackPanel.Triggers>
    <EventTrigger RoutedEvent="MouseEnter">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimation To="74" Storyboard.TargetProperty="Height" Duration="0:0:0.27"></DoubleAnimation>
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
    <EventTrigger RoutedEvent="MouseLeave">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimation To="0" Storyboard.TargetProperty="Height" Duration="0:0:0.27"></DoubleAnimation>
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
</StackPanel.Triggers>
```

### Binding 變數觸發

```xml
<StackPanel.Styles>
    <Style TargetType="StackPanel">
        <Setter Property="Height" Value="74" />
        <Style.Triggers>
            <DataTrigger Binding="{Binding TopVisibility}" Value="Visible">
                <DataTrigger.EnterActions>
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation To="74" Storyboard.TargetProperty="Height" Duration="0:0:0.27"></DoubleAnimation>
                        </Storyboard>
                    </BeginStoryboard>
                </DataTrigger.EnterActions>
                <DataTrigger.ExitActions>
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation To="0" Storyboard.TargetProperty="Height" Duration="0:0:0.27"></DoubleAnimation>
                        </Storyboard>
                    </BeginStoryboard>
                </DataTrigger.ExitActions>
            </DataTrigger>
        </Style.Triggers>
    </Style>
</StackPanel.Styles>
```
