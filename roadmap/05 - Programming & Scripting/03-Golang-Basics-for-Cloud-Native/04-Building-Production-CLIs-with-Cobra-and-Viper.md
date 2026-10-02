# 04 - Building Production CLIs with Cobra and Viper

## 1. What is Cobra?

**Cobra** is the industry standard Go library that powers `kubectl`, `hugo`, `docker`, and `gh`.

```go
package main

import (
	"fmt"
	"os"

	"github.com/spf13/cobra"
)

var rootCmd = &cobra.Command{
	Use:   "cloudctl",
	Short: "cloudctl is a multi-cloud automation tool",
}

var deployCmd = &cobra.Command{
	Use:   "deploy [app-name]",
	Short: "Deploy an application to the cloud",
	Args:  cobra.ExactArgs(1),
	Run: func(cmd *cobra.Command, args []string) {
		env, _ := cmd.Flags().GetString("env")
		fmt.Printf("Deploying %s to %s environment...\n", args[0], env)
	},
}

func init() {
	deployCmd.Flags().StringP("env", "e", "dev", "Target environment (dev|staging|prod)")
	rootCmd.AddCommand(deployCmd)
}

func main() {
	if err := rootCmd.Execute(); err != nil {
		os.Exit(1)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Concurrency](./03-Concurrency-Goroutines-Channels-and-WaitGroups.md) | [README](./README.md) | [05 - HTTP & Context](./05-HTTP-Services-and-Context-Cancellation.md) |
