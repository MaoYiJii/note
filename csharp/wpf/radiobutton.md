# RadioButton

### 當 RadioButton 與自己的 CommandParameter 相等時選取

#### 在 Resources 加入 Style

參考 MultiValueConverter 的 EqualsMultiValueConverter

```xaml
<Style x:Key="RadioButton_OnCreate" TargetType="RadioButton" BasedOn="{StaticResource {x:Type RadioButton}}">
    <Style.Triggers>
        <DataTrigger Value="True">
            <DataTrigger.Binding>
                <MultiBinding Mode="OneWay" Converter="{StaticResource EqualsMultiValueConverter}">
                    <Binding Path="Settings.StateOnCreate"></Binding>
                    <Binding Path="CommandParameter" RelativeSource="{RelativeSource Self}"></Binding>
                </MultiBinding>
            </DataTrigger.Binding>
            <Setter Property="IsChecked" Value="True" />
        </DataTrigger>
    </Style.Triggers>
</Style>
```

#### 在 RadioButton 引用 Style

```xaml
<RadioButton Margin="3, 0" VerticalAlignment="Center" GroupName="{Binding Column.Name, StringFormat=OnCreate_{0}}"
             Style="{StaticResource RadioButton_OnCreate}"
             Content="可編輯"
             CommandParameter="{x:Static services:GenerateColumnState.Write}"
             Checked="ColumnSettings_Checked"></RadioButton>
<RadioButton Margin="3, 0" VerticalAlignment="Center" GroupName="{Binding Column.Name, StringFormat=OnCreate_{0}}"
             Style="{StaticResource RadioButton_OnCreate}"
             Content="唯讀"
             CommandParameter="{x:Static services:GenerateColumnState.Read}"
             Checked="ColumnSettings_Checked"></RadioButton>
<RadioButton Margin="3, 0" VerticalAlignment="Center" GroupName="{Binding Column.Name, StringFormat=OnCreate_{0}}"
             Style="{StaticResource RadioButton_OnCreate}"
             Content="隱藏於 &lt;hidden&gt;"
             CommandParameter="{x:Static services:GenerateColumnState.Hidden}"
             Checked="ColumnSettings_Checked"></RadioButton>
```
