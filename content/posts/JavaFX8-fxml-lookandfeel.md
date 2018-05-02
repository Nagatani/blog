---
title: JavaFX8でのLook and Feelを設定する
date: 2014-10-08 17:00:56
categories:
- JavaFX8
tags:
- Java
- JavaFX8
- FXML
---
# JavaFX8のLook and Feel
標準では、Modenaよ呼ばれるルック・アンド・フィールが使用されている。

これを変更することで、アプリケーション全体の外観を変える事ができる。
詳細は省く。

FXMLには、全体の外観を変更する手法として、HTMLと同様にCSSを使用することができるが、これに関してはまた後日記事にしようと思う。
<!-- more -->

# メインクラスでコーディングが必要

以下、Hello Worldのアプリで作成した、JavaFXApplication1.javaのコードをまるごと載せる。

```java
package javafxapplication1;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;

/**
 *
 * @author Nagatani
 */
public class JavaFXApplication1 extends Application {

    @Override
    public void start(Stage stage) throws Exception {
        Parent root = FXMLLoader.load(getClass().getResource("FXMLDocument.fxml"));

        Scene scene = new Scene(root);

        //setUserAgentStylesheet(STYLESHEET_MODENA);
        setUserAgentStylesheet(STYLESHEET_CASPIAN);

        stage.setScene(scene);
        stage.show();
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        launch(args);
    }

}
```

ここで、21,22行目に追加した`setUserAgentStylesheet()`メソッドを`public void start(Stage stage) throws Exception`メソッド内でコールする。
`setUserAgentStylesheet()`メソッドの引数には、`STYLESHEET_CASPIAN`と`STYLESHEET_MODENA`が用意されている。
標準ではどうやらこの2種類のようですね。

`STYLESHEET_MODENA`は、なにも設定しなくても適用されているスタイルのため、あえて明示的に書かなくても良い。
以前まで使用されていた標準のスタイルにしたい場合は、`STYLESHEET_CASPIAN`を設定する。


# 実際どうなるか
テキトーにコントロールを乗せたものをMacで実行した場合は以下のようになる。
※FXMLは最後に載せます。

まずは標準のスタイルのModenaから。
![STYLESHEET_MODENA](/image/sc-javafx8-laf-modena.png)

次に、Caspian。
![STYLESHEET_CASPIAN](/image/sc-javafx8-laf-caspian.png)

# どっちがいいか？
好みによると思う。

スクリーンショット見るとなんとなく、Modenaの方が余白を大きく取ってあり、各コントロールに余裕があるように感じますかね。
逆にCaspianは、圧迫感があるようなないような……。

個人的には、Modenaの方がスッキリしてて余裕あって好きです。

# スクリーンショット用に作成したFXML
だいぶ雑に作成したので、細かいところへのツッコミはなしでお願いします。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.image.*?>
<?import javafx.scene.web.*?>
<?import javafx.scene.text.*?>
<?import java.lang.*?>
<?import java.util.*?>
<?import javafx.scene.*?>
<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<Accordion maxHeight="-Infinity" maxWidth="-Infinity" minHeight="-Infinity" minWidth="-Infinity" prefHeight="400.0" prefWidth="600.0" xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1">
  <panes>
    <TitledPane animated="true" text="Controls - Button">
      <content>
        <VBox AnchorPane.bottomAnchor="0.0" AnchorPane.leftAnchor="0.0" AnchorPane.rightAnchor="0.0" AnchorPane.topAnchor="0.0">
           <children>
              <HBox>
                 <children>
                    <Button mnemonicParsing="false" text="Button" />
                    <Button defaultButton="true" mnemonicParsing="false" text="Default Button" />
                    <Button cancelButton="true" mnemonicParsing="false" text="Cansel Button" />
                    <Button disable="true" mnemonicParsing="false" text="Button Disabled" />
                 </children>
              </HBox>
              <HBox>
                 <children>
                    <MenuButton mnemonicParsing="false" text="MenuButton">
                      <items>
                        <MenuItem mnemonicParsing="false" text="Action 1" />
                        <MenuItem mnemonicParsing="false" text="Action 2" />
                      </items>
                    </MenuButton>
                    <MenuButton mnemonicParsing="false" text="MenuButton Disabled">
                      <items>
                        <MenuItem mnemonicParsing="false" text="Action 1" />
                        <MenuItem mnemonicParsing="false" text="Action 2" />
                      </items>
                    </MenuButton>
                 </children>
              </HBox>
              <HBox>
                 <children>
                        <Group />
                    <RadioButton mnemonicParsing="false" selected="true" text="RadioButton - Selected" />
                    <RadioButton mnemonicParsing="false" text="RadioButton - not Selected" />
                    <RadioButton disable="true" mnemonicParsing="false" text="RadioButton Disabled" />
                 </children>
              </HBox>
              <HBox>
                 <children>
                    <CheckBox mnemonicParsing="false" text="CheckBox" />
                    <CheckBox mnemonicParsing="false" text="CheckBox" />
                    <CheckBox mnemonicParsing="false" text="CheckBox" />
                 </children>
              </HBox>
              <HBox>
                 <children>
                    <ChoiceBox prefWidth="150.0" />
                 </children>
              </HBox>
              <HBox>
                 <children>
                    <ComboBox prefWidth="150.0" />
                    <ComboBox prefWidth="150.0" />
                 </children>
              </HBox>
                  <HBox>
                     <children>
                        <ToggleButton mnemonicParsing="false" text="ToggleButton" />
                     </children>
                  </HBox>
                  <Slider />
                  <Slider orientation="VERTICAL" />
                  <ScrollBar />
                  <ScrollBar orientation="VERTICAL" />
           </children>
        </VBox>
      </content>
    </TitledPane>
    <TitledPane text="Controls - Text">
      <content>
        <VBox AnchorPane.bottomAnchor="0.0" AnchorPane.leftAnchor="0.0" AnchorPane.rightAnchor="0.0" AnchorPane.topAnchor="0.0">
           <children>
              <HBox>
                 <children>
                    <Label text="Label" />
                 </children>
              </HBox>
              <HBox>
                 <children>
                    <TextField text="TextField" />
                    <TextField promptText="Prompt Text" />
                    <TextField editable="false" text="Not Editabled" />
                    <TextField disable="true" text="Disabled" />
                 </children>
              </HBox>
              <HBox prefHeight="100.0" prefWidth="200.0">
                 <children>
                    <TextArea prefHeight="200.0" prefWidth="200.0" text="Text Area" />
                        <DatePicker promptText="Date Picker" />
                 </children>
              </HBox>
                  <HBox>
                     <children>
                        <TextFlow prefHeight="200.0" prefWidth="200.0">
                           <children>
                              <Label text="TextFlow" />
                           </children>
                        </TextFlow>
                     </children>
                  </HBox>
           </children>
        </VBox>
      </content>
    </TitledPane>
    <TitledPane text="Controls - Other">
      <content>
      </content>
         <content>
            <VBox>
               <children>
                  <HBox>
                     <children>
                        <TabPane tabClosingPolicy="UNAVAILABLE">
                          <tabs>
                            <Tab text="Untitled Tab 1">
                              <content>
                                <AnchorPane minHeight="0.0" minWidth="0.0" />
                              </content>
                            </Tab>
                            <Tab text="Untitled Tab 2">
                              <content>
                                <AnchorPane minHeight="0.0" minWidth="0.0" />
                              </content>
                            </Tab>
                              <Tab text="Untitled Tab">
                                <content>
                                  <AnchorPane minHeight="0.0" minWidth="0.0" />
                                </content>
                              </Tab>
                              <Tab text="Untitled Tab">
                                <content>
                                  <AnchorPane minHeight="0.0" minWidth="0.0" />
                                </content>
                              </Tab>
                              <Tab text="Untitled Tab">
                                <content>
                                  <AnchorPane minHeight="0.0" minWidth="0.0" />
                                </content>
                              </Tab>
                          </tabs>
                        </TabPane>
                     </children>
                  </HBox>
                  <HBox>
                     <children>
                        <TitledPane animated="false" text="TiledPane">
                          <content>
                            <AnchorPane minHeight="0.0" minWidth="0.0" />
                          </content>
                        </TitledPane>
                     </children>
                  </HBox>
                  <HBox>
                     <children>
                        <ToolBar>
                          <items>
                              <Label text="ToolBar" />
                            <Button mnemonicParsing="false" text="Button" />
                          </items>
                        </ToolBar>
                     </children>
                  </HBox>
                  <HBox>
                     <children>
                        <Label text="ListView" />
                        <ListView />
                     </children>
                  </HBox>
               </children>
            </VBox>
         </content>
    </TitledPane>
  </panes>
</Accordion>
```
